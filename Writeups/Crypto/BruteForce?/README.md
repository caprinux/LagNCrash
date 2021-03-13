# BruteForce?

---

### Challenge Description

Do we really need to brute force a password out?

[brute_force.zip](https://github.com/caprinux/LagNCrash/files/6134289/brute_force.zip)

---

### Solution

We are given a zip and unzipping it gives us 2 files, readme.txt and Desktop.zip.

If you try to unzip Desktop.zip, it prompts for a password. But you can see that there is also a readme.txt and answer/flag.txt inside.

Ohhhh trust me when they tell you not to brute force, you don't brute force.

I tried brute forcing with many many tools but it all ended up in vain.

Eventually, even after all the useless hints we have bought till this point, we finally succumbed and bought the hint this time.

```
The zip was encrypted with legacy encryption, can some sort of attacks break this?
```

omg. is this a useful hint?!??! the lnc committee is giving us a USEFUL hint!?!?!? 

I searched "break legacy encryption zip" on google and found [bkcrack](https://github.com/kimci86/bkcrack). _i found pkcrack too but i eventually used bkcrack_

The idea is that you need at least 12 bytes of whatever is inside the encrypted zip, and the larger the number of bytes of plaintext we have, the faster the attack.

Here we make our first sound assumption that the readme.txt we have in plaintext is the same readme.txt inside Desktop.zip. 

In fact, this assumption has to be true if we are trying to break legacy encryption with this method + I was not able to find any other ways to break legacy encryption.

However after trying to decrypt the readme.txt in Desktop.zip with our own readme.txt I realised there was a problem. 

In the same github for bkcrack there was a [tutorial.md](https://github.com/kimci86/bkcrack/blob/master/example/tutorial.md) that explained this problem.

If you do unzip -Z desktop.zip, you can see that the readme.txt inside the zip is deflated.

![image](https://user-images.githubusercontent.com/76640319/111022038-2e3a2000-840b-11eb-9a6c-5212cc1c8fa6.png)

But the readme.txt that we have is not deflated. We cannot simply use our readme.txt to break into Desktop.zip.

However if you remember, we actually had another zip before this, brute_force.zip. And inside brute_force.zip is a deflated readme.txt that is not encrypted.

So putting all of these together, we start to crack our zip.

![image](https://user-images.githubusercontent.com/76640319/111022457-d51fbb80-840d-11eb-8160-2889eb5c10a4.png)

_(disclaimer: how fast the keys are cracked depends on how much prcoessing power and memory u allocated to your computer. I ran this on my laptop with 8 cores and 6gb memory allocated to my linux)_

After a short wait, we got the key. With the key we can now extract flag.txt

However, after extracting it is still deflated and we can use a tool that came with bkcrack to inflate it.

![image](https://user-images.githubusercontent.com/76640319/111022588-8161a200-840e-11eb-8c5b-4ba229597936.png)

```flag
LNC{plain_t3xt_BRO0T_F0RCE}
```

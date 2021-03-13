# Hellman

---

### Challenge Description

Hellman loves sharing, find his secret

[Hellman.zip](https://github.com/caprinux/LagNCrash/files/6134215/Hellman.zip)

---

### Solution

We are given a text file with the following text:

```
17746761831:1219113036371115975795111736303119121

John's Public:OTI5OTc0MTE4MDE1OTYyNDA3MzQxNjMyNTc1ODY4NTIzODE0
Richard's Public: Nzk4NDM0Njg2OTEzMzczNzI0NTE0NzA3MTg4NjIyMDQ4NjA2

What is their shared Secret key?
LNC{Insert_Secret_Key}

Hint: This can be done in a diffie.
```

If you google Hellman Cipher, it brings you to [Diffie Hellman Key Exchange](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange).

We know that we are at the right place because the hint also mentioned "This can be done in a **diffie**"

Most of you who tried this challenge has probably reached here too.

Reading the wikipedia for Diffie Hellman Key Exchange, we can see how this key exchange is done.

![image](https://user-images.githubusercontent.com/76640319/111020213-2e342300-83ff-11eb-999f-51b94175f584.png)

Comparing with the text file that we were provided, we can rearrange the values to obtain. 

However, our public keys have letters, and we only want numbers since we are going to be doing calculations.

If you plug A or B into cyberchef, you will see that it is actually base64 encoded with the magic function. 

After decoding our public keys and rearranging our variables, we get

```
g = 17746761831
q = 1219113036371115975795111736303119121
A = OTI5OTc0MTE4MDE1OTYyNDA3MzQxNjMyNTc1ODY4NTIzODE0
B = Nzk4NDM0Njg2OTEzMzczNzI0NTE0NzA3MTg4NjIyMDQ4NjA2
```

We want to find the secret key a, or the secret key b, either one will work.

The relationship that we can use here is A = g^(a) mod p. Since we only have one unknown which is a.

However, we come to another problem, how do we find a? 

We arrive at the problem of a Discrete Logarithm Problem.

However my brain is not big enough to handle this so I resort to googling

```
diffie hellman writeups
diffie hellman script
diffie hellman attack ctf
```

Although these searches weren't conclusive in helping us obtain a, I realised that sage played a huge role in most of these writeups, although some of them are too slow when I copy and run it in my sage. 

Hence I continue my search and upon searching for ``diffie hellman attack ctf sage``

I arrive at this [git repository](https://github.com/ValarDragon/CTF-Crypto). 

We navigate to the Diffie Hellman sage script, copy and paste it into our [cryptohack docker and run it with sage](https://github.com/cryptohack/cryptohack-docker)

```
g= 17746761831
p= 1219113036371115975795111736303119121
A = 929974118015962407341632575868523814
k = GF(p)
AInField = k(A)
gInField = k(g)
discrete_log_lambda(AInField,gInField,(1,2**46))
```

I left the script to run for a few minutes and tbh I wasn't expecting anything when I came back but to my surprise there was an output.

_what you do not see are the constant failures of scripts taking half an hour or longer to run and me just giving up on them_

a = 11735640203

Now the rest is easy. 

We take B^a mod p to give us the shared secret key.

![image](https://user-images.githubusercontent.com/76640319/111020609-2aee6680-8402-11eb-8cb3-bbc83efa56f5.png)

```
LNC{1145928362990075408502981475452396432}
```

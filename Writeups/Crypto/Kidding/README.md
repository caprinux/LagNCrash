# Kidding
---

My friend told me a joke the other day, it was about interpoly but it really wasn't that funny at all... The weird thing is that he probably wasnt keyeding?

Files: unknown.zip

SHA1: 876d1096b3a50f85c348a97ba988694444561bb9

---

### Solution

The zip file can be very easily broken with brute force attack.

```
$ fcrackzip -u -D -p rockyou.txt kidding.zip


PASSWORD FOUND!!!!: pw == stargazer
```

Now that we have the password, we can unzip the zip file and we will obtain joke.txt.txt.

```
$ unzip kidding.zip 
Archive:  kidding.zip
[kidding.zip] joke.txt.txt password: 
extracting: joke.txt.txt    
$ cat joke.txt.txt
DGE{a_w4qgs_c1rr1gl!}
```

This definitely looks like some sort of substitution cipher.

After googling for some time, I couldn't seem to find what cipher this was.

So since I know that the start has to be LNC{, I used a manual subsitution solver and I got this.

![image](https://user-images.githubusercontent.com/76640319/111021466-73f4e980-8407-11eb-813c-e631126cab71.png)

Since the numbers would also stay the same in a substitution cipher, we have most of the letters out.

After reading the challenge description once again and considering the letters we already have as well as recurring letters, we managed to make out the rest of the flag.

```
LNC{i_w4snt_k1dd1ng!}
```

_p.s. this was not done the intended way_
_the intended solution was to figure out that it was a keyed caesar cipher and guess that the key was interpoly_

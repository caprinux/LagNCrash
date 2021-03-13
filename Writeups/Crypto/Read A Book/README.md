# Let's Read a Book

---

### Challenge Description

I reccomend you read this book, its pretty interesting! 4686490241879

Can you solve this: ‡44)46*:

PS Take note after decryption, the output is NOT in flag format LNC{}. The final flag entered should be in all caps eg LNC{THIS_IS_THE_FLAG}.

---

### Solution

My teammates were working on this challenge before me, and left it hanging.

They told me : 

```
4686490241879
tot this was an ISBN 
but I'm wrong
```

As a JC student, I was just chilling on the MRT with nothing much to do and I figured that I might as well check this crypto challenge out.

After googling up on ISBN (yeah idk whats ISBN), I figured out that it's an International Standard Book Number, and contains 10 or 13 digits.

Counting the number of digits in 4686490241879, there is indeed 13 digits and seems like an ISBN.

The title of the challenge also hints to ISBN so I was thinking it can't be such a coincidence that the name is **Read a Book** and the number has 13 digits.

It definitely has to be ISBN.

However, after using many ISBN Searches that I found on google, none of them yield a result for 4686490241879.

Also seeing by how many solves there was, there's clearly something I wasn't seeing.

Suddenly a thought hit me, what if I reverse the numbers of the ISBN and search for that?

So I reversed the numbers provided and I got 9781420946864

Searching it up on ISBN, I got this:

![image](https://user-images.githubusercontent.com/76640319/111019346-97189c80-83f9-11eb-8f99-4fb106e594ef.png)

This must be it!! Looking at the synopsis of the book did not yield much results however, it was merely a fiction about someone being bitten by a gold bug.

However, after more googling, I find the Gold Bug Cipher.

I go to an online [gold bug cipher decoder](https://www.dcode.fr/gold-bug-poe) and copy paste the ciphertext provided into it.

Voila.

```
OHHSHINY
flag: LNC{OHHSHINY}
```

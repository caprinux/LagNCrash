# Split Up Guys
---

### Challenge Description

The time traveller has escaped! After getting caught, the time traveller managed to escape from his cell, but not for long. One of our officer has records of him going somewhere, but where could it be?

[escaped.zip](https://github.com/caprinux/LagNCrash/files/6133938/escaped.zip)

---

### Solution

This challenge gave me some deja vu on a WhiteHacks challenge by one of the sponsors (GovTech?) which gave a pcap file and required you to extract all files from the pcap file and piece together the extra bytes of hex codes to form a picture.

In this case, analysing the pcap file, we see that there are a bunch of downloads made with http. We can easily download this through File>Export Objects>HTTP.

We obtain a bunch of .cab files which I think are downloads from microsoft? Anyways, these are useless, so we move on.

If you open most of the files, you either see a picture of a train or get some link that sends you to a youtube premium subscription page. 

However, if you read the next.py file you will get

```
word=shop-the-beat.neocities.org
```

Going to this link, we started analyzing the website, looking at the source code and all.

At the bottom of the source code of the index page, we find the first half of the flag. ``LNC{G0t-ya``

However, we are far from done. 

From the source code of the product page, we get a key.

``` Key:MIICdwIBADANBgkqhkiG9w0BAQEFAASCAmEwggJdAgEAAoGBAL6O5ns5KLLIp7HTgvOOub0LChwJzWOKms9jA9CuxeGsXV1s4qLFBrGLq35Enxq1YT3Jdp33Ov3Olyj4COCO3vrpvPAZDNWVy2ML3AEAjIzZfoDTS3LH7wh9JfRjfjSn0ncFd09si3u5dYjnuMbuul+woyihu5wsGyW9kOI5oSXzAgMBAAECgYEArh5O1WtOF8anDCKzNi9E4kqzCxmd1YWxnSvhMY5w+5sOmK5Ei+wyCIsRwUechcAUQWvTedWMzps1nda97co+TTzQjXC6/xU0EKNc7L9W3AdMdqwZmrl67qwYDsgx0znf/GYVp0T5Lw42rg1KVF3d6ggS7P4t3Nw4O08lQaKT3WECQQDl0Tgijym5qF6G+6oYO7Ge1Qd4TDi5YCaHMR2kUafKX4zuMM/cA2d6yJd6pGU7ehuMvmJ4Qo4SRINR+Upc/BIRAkEA1ESpySX6nyhS2AS5Oz5450WN9WRNKm/YccqkDX5m5U9JDjdn8UgPj98voKSgQE7OTvBXiYzGh0p56bt1B6YzwwJBAJaEZ9qinzZcosSPkYDbn+KiLYlJiqFG6xUCQyK65EU0PY5HY+v6Qsz1EdkeULsap26Pxthy5q/qNYP73Qt3gLECQHxj5G9Iv05nbezDD91E5cr5epAUABhfRKKiUnLJ4Ph99tzK4TGGvf/clWd5MaOdys59j36+rVR482ph/NnHrssCQHY6AmpiJ5/a4GkiySKbPHnVRwHptLtVdcEUD6cO0s9vEt2geiIzj7PC+BiqMZcGiXPdrBF/y9h5Sxg4GqVgCC0=
```

Looks like a RSA key, but we do not have the cipher text.

I got hard stuck here for some time, and created many support tickets once again (thansk for tolerating my shit Support :p)

Anyways, I eventually come to a payment page and after filling in all the details with junk and submitting the form, I get a few javascript prompts and eventually land on a 405 Not Allowed page.

We further analyse this by looking at the Network Traffic in Inspect window. Scrolling for abit, we see this.

![Screenshot 2021-03-13 122424](https://user-images.githubusercontent.com/76640319/111018943-122c8380-83f7-11eb-8ccc-727feb98e9bc.png)

Flag: flag.html

If we append this file to the end of our browser, shop-the-beat.neocities.org/flag.html, we arrive at this page.

![image](https://user-images.githubusercontent.com/76640319/111018976-3ee09b00-83f7-11eb-853d-8443b471edb0.png)

So the page provided us with an encrypted text, 

``` 
PwmiWalXGYqGmE4cwb5Che/+rJFGgYj6SlMN6DCZOZa2B2ZECxbkjIEv1r6fn74qiG6S8MREWuTrFsiT2S0Oexs+cU+wVj54IEEE7rHxpmUOsgsUU8VZx1QzyJH/EW1Hz2yDBHTptsQrc+umf4FcWBr/kb5F0wfIpnthJZ0fbLE=
```

Remember the long key we got earlier that I suspected was a RSA key? If we plug the ciphertext and the key into a [RSA Decoder](https://www.devglan.com/online-tools/rsa-encryption-decryption), we get the second half of the flag.

Putting our 2 halves of the flag together, we get the final flag.

```
LNC{G0t-ya_cr1minal!}
```

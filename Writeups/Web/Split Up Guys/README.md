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

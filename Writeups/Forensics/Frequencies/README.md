# Frequencies
---

### Challenge Description

We’ve managed to intercept a series of beeps, can you find out what they mean?

[Frequencies.zip](https://github.com/caprinux/LagNCrash/files/6134327/Frequencies.zip)

---

Initially I thought this was morse code of sorts. 

So I tried using an online Morse Code Adaptive Audio Decoder to decode this audio and I got ``I I I S I I I I S I I S I I S S S S S``. 

Trying to put it into binary or whatever doesnt work, but I couldn't figure what I wasn't seeing here.

I was doing this at 12am of Day 3 and I just gave up and went to sleep.

When I woke up I saw my friend's messages:

![image](https://user-images.githubusercontent.com/76640319/111022750-bae6dd00-840f-11eb-891d-554ff7649907.png)

Apparently he figured out what it was but he didn't have the tools to do it. 

But he still managed to decipher most of it correctly just from listening to it !?!?!?! Insane. My teammates are actually gods.

Anyways searching for dtmf dial tone decoder online leads me to [this](https://github.com/ribt/dtmf-decoder). 

After getting the tool, I plugged the audio into the tool and I got this string of numbers.

```
$ dtmf Frequencies.wav 

7678671236884777011595971145195107101119108125
```

After counting the number of beeps and separating the numbers, I got this

```
76 78 67 123 68 84 77 70 115 95 97 114 51 95 107 101 119 108 125
```

If we convert this from decimal to text in cyberchef, we get the flag.

```
LNC{DTMFs_ar3_kewl}
```

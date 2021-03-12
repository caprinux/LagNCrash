# Tedious? Or is it?
---

### Challenge Description

My friend pranked me and hide my secret in one these!

[tedious.zip](https://github.com/caprinux/LagNCrash/files/6128364/tedious.zip)

---

### Solution

```
$ file tedious
tedious: Zip archive data, at least v2.0 to extract
```

We see that we are given a zip file, unzipping it with `unzip tedious` will provide us with thousands of .zip file in the directory QR.

We open one of these QR codes with zbarimg, and we are presented with a string of text.

```
$ zbarimg 1.zip 

QR-Code:LNC{IS_THIS_TH3_R3AL_0N3?}

scanned 1 barcode symbols from 1 images in 0.02 seconds
```

If you try to submit this flag, it doesn't work.

The rest of the few thousand QR codes are probably also the same fake flag, with 1 of them standing out.

With that knowledge, I make a bash script to loop through all the QR codes.

```
for i in {1..4255}
do
zbarimg --quiet $i.zip
done
```

With that, I execute my script and pipe it through an inverse grep search such that only flags that are not the fake flags will be printed

```
./script | grep -v "LNC{IS_THIS_TH3_R3AL_0N3?}"
```

After waiting for sometime, we have the flag.

```
QR-Code:LNC{IS_THlS_TH3_R3AL_0N3?}
```






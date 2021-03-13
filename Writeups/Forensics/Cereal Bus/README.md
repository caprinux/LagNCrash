# Cereal Bus

---

### Challenge Description

Oh no! You can't seem to recall your login credentials to the best cereal site on Earth! Luckily, you remembered you so happen to have a file lying around that may just help...

NOTE: Submit your flag as LNC{username:password}

Hint: Huh. You found a document with [this URL](https://www.usb.org/sites/default/files/hut1_21.pdf) on it...

[cereal.zip](https://github.com/caprinux/LagNCrash/files/6134385/cereal.zip)

---

### Solution

Yeah we spent 10 points on the hints and they gave us a 319 pages document. Nice.

I clearly spent the 10 points and then not read any of the 320 pages. Anyways;

We are given a bzcat file, which I extracted to get a pcapng file. 

```
$ bzcat file.bz2 > hi
$ file hi
hi: pcapng capture file - version 1.0
```

We can open this pcapng file in wireshark.

![image](https://user-images.githubusercontent.com/76640319/111024319-23868780-8419-11eb-90bb-582448f3de24.png)

We basically see a bunch of USB and USBHUB protocols. I personally have never seen something like this before; so I got right into googling.

I googled ``ctf usb usbhub pcapng`` and it brought me to this [writeup](https://abawazeeer.medium.com/kaizen-ctf-2018-reverse-engineer-usb-keystrok-from-pcap-file-2412351679f4).

So basically I followed the writeup instructions; 

Firstly, I filtered to get the relevant packets with keystroke data; ((usb.transfer_type == 0x01) && (frame.len == 72)) && !(usb.capdata == 00:00:00:00:00:00:00:00)

![image](https://user-images.githubusercontent.com/76640319/111024409-9ee83900-8419-11eb-9280-9e92e6410508.png)

I then add Leftover Capture Data as a column

![image](https://user-images.githubusercontent.com/76640319/111024428-bf17f800-8419-11eb-8506-729011562b8f.png)

I then exported the packets as csv.

![image](https://user-images.githubusercontent.com/76640319/111024469-e66ec500-8419-11eb-9bf6-2d176aa98f05.png)

I filtered out all the other extra information and only printed my Leftover Capture Data. However, I couldn't manage to remove the quotation marks and got many errors when I tried. 

![image](https://user-images.githubusercontent.com/76640319/111024557-66952a80-841a-11eb-95a3-b2919331aed5.png)

Hence I used Notepad to replace all the Quotations. 

![image](https://user-images.githubusercontent.com/76640319/111024643-dc999180-841a-11eb-9482-a29f32f51673.png)

Now I have all the leftover capture data.

```
0800000000000000
 08001e0000000000
 08001e0000000000
 00001e0000000000
 0100000000000000
 0300000000000000
 0300130000000000
 0300000000000000
 0200000000000000
 0000040000000000
 0000180000000000
 0000170000000000
 00000b0000000000
 0000370000000000
 0000170000000000
 00000b0000000000
 0000080000000000
 0000050000000000
 0000080000000000
 0000160000000000
 0000170000000000
 0000060000000000
 0000080000000000
 0000150000000000
 0000040000000000
 0000080000000000
 00002a0000000000
 00002a0000000000
 0000080000000000
 0000040000000000
 00000f0000000000
 0000160000000000
 0000370000000000
 0000060000000000
 0000120000000000
 0000100000000000
 0000280000000000
 00002b0000000000
 0200000000000000
 0200060000000000
 0000060000000000
 0000080000000000
 0000150000000000
 0000080000000000
 0000040000000000
 00000f0000000000
 0200000000000000
 0200100000000000
 0200000000000000
 0000040000000000
 0000110000000000
 00002b0000000000
 0200000000000000
 0200170000000000
 0200000000000000
 0200000000000000
 0200340000000000
 0200000000000000
 0000190000000000
 0000250000000000
 0200000000000000
 0200240000000000
 0200000000000000
 00000f0000000000
 00002e0000000000
 00001b0000000000
 0200000000000000
 02001d0000000000
 0200000000000000
 0200000000000000
 02002d0000000000
 0200000000000000
 0000160000000000
 0200000000000000
 0200150000000000
 0200000000000000
 0000300000000000
 00002a0000000000
 0200000000000000
 0200300000000000
 0200000000000000
 0000180000000000
 0000190000000000
 00002a0000000000
 0200000000000000
 0200190000000000
 0200000000000000
 0200000000000000
 0200310000000000
 0200000000000000
 0200000000000000
 02000f0000000000
 0200000000000000
 0000260000000000
 0200000000000000
 02001f0000000000
 0200000000000000
 0000050000000000
 0200000000000000
 0200350000000000
 0200000000000000
 0200000000000000
 0200210000000000
 0200000000000000
 0000330000000000
 0000070000000000
 00002a0000000000
 0200000000000000
 0200070000000000
 0200000000000000
 00002b0000000000
 00002b0000000000
 00002b0000000000
 00002b0000000000
 0200000000000000
 02002b0000000000
 0200000000000000
 0200000000000000
 02002b0000000000
 0200000000000000
 00002b0000000000
 0000280000000000
```

I tried the python code used in the referenced github earlier, however it didn't work for me for some reason.

After googling for some time, I found this [website](https://blog.stayontarget.org/2019/03/decoding-mixed-case-usb-keystrokes-from.html)

I copy pasted the script into my directory and ran it.

![image](https://user-images.githubusercontent.com/76640319/111024754-76613e80-841b-11eb-9bc5-654d41d2bc28.png)

I now run it with Leftover Capture Data that I got earlier.

![image](https://user-images.githubusercontent.com/76640319/111024813-a7417380-841b-11eb-8826-8e7850189ed8.png)

```
111pauth.thebestceraedeldeleals.comEntertabCcerealMantabT"v8&l=xZ_sR]del}uvdelV|L9@b~$;ddelDtabtabtabtabtabtabtabEnter
```

pauth.thebestcereal.com looks like the website that this person was logging in to.

CcerealMantabT"v8&l=xZ_sR]del}uvdelV|L9@b~$;ddelD looks like the username and the password. Interpreting the keystrokes, we get:

```
CcerealMan:T"v8&l=xZ_sR}uV|L9@b~$;D
```

However, if you tried submitting this, the ctf site would say flag is invalid.

Now you DOS the website...

or maybe you big brain and wonder why this guy username got 2 Cs and then u delete one of them

```
LNC{CerealMan:T"v8&l=xZ_sR}uV|L9@b~$;D}
```

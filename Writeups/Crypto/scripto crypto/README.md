# scripto crypto

---

### Challenge Description

Can you fix the script to get the flag?

NOTE: Read the comments in the file before you attempt to fix the code

[challenge.zip](https://github.com/caprinux/LagNCrash/files/6134249/challenge.zip)

---

### Solution

A python file was attached. 

This python file consists of a table of ciphertext followed by a short script to decrypt each ciphertext.

Looking at the code, 

```py
key =  b'i want 16 bytezzi want 16 bytezz'
cipher = AES.new(key, ?)

while count < 25:
	string1 = table[count]
	count = count + 1
	rotten = codecs.encode(str(string1), 'rot-13')
	for i in range(999):
		rotten = cipher.decrypt(rotten)
	print(rotten)

```

We can see that it first decrypts the ciphertext with AES, but the mode was not specified.

However, we are provided with a key and since we are not given an IV, it is likely ECB, since ECB does not use an IV.

Then it goes through every element in the table and decrypts it with rot13.

Looking back at the table,

```
table = [b'J+TRKJnPo0EAaLH5oio7qW6lC92FUFY96kZPBFqj4NU=',
         b'UERMnI8D1rOakgIExgaBrSqxzgWkSl/PY58t5MuryL4=',
         b'CFXKWz5CUJJyPrQB1Axqo4K7xMEr6xKIxpCz9Q1+Lc8=',
         b'mSzAc6NvQHOiZyCcfa7m/E+CIuDAJHNrYdHqCWO9qs8=',
         b'r/81JVwz7QtfBjZoYoxhx1PFbc+32WmzAfEiROMfs+4=',
         b'mFcLrpE/mJu4kfrgIMjnAhzrYHjyjyUZCOrRbBXjzFw=',
         b'QZV3lswJ5cjD0vPOJD2RNsGmpmJfEidHHdyECpx/AGk=',
         b'H2wx879JqZVmousKnHip3iSMw6MvxeMqgxIY3ZseToY=',
         b'OUUHem3lwgSA2PqLDkp7XY8inlI1fYw0l0wYKVBJ7uA=',
         b'NTAl0vlCxoxlwZCddxSJS5c80ALp0I4DMPwJCb1qer4=',
         b'59meCW3BGFMrVoKS40Net4bogld8uNDLpWmouuTu8iU=',
         b'3dX4EHGlVY8FXEVS4G1KvKhhsnunCoPgmHw9raybrsw=',
         b'3fxahhfCY27tjTduqeW+f8qxtnRCQkabDzzwdixqFUc=',
         b'xbAbq0HnibOd2uKAvmazFvq+5SWzyvKG1gIB/uUqLu4=',
         b'hiw6A01RyDcOJTVSJ1/XxIQLZI40tgX5cOHlFsVFwTM=',
         b'Rq+xfITYIhhdsUpchEBJ7Dinn9wi42+mtjuQhWSY3YM=',
         b'BpUeSFVAnsoFwDiBfVuvRGOe3UKJrnl3Z2wlQ2WDigw=',
         b'+O1srpp/thvcvm7nhdbn4M6Xg0DiVCuxTezRtPX6M6A=',
         b'7BTnOYJA3NyCE+SGGeH6mq+WXbFM+QLd4DmIqVYhZ1w=',
         b'EynIrUqnKQX0rv9cqgEhwkxxUwCb2GLUgN4pTpmKESM=',
         b'GGDLLuW08z4YaXVke4FGf9CcsGa0x+fbclto0ZOT+7o=',
         b'er2/8Q4XeF05y3q4DtgxrC5tfpawfZR5qg55qlcLPA8=',
         b'VptI/j/Vnyrh/H8DH/lyRjgZqIvG1vxXvhV3xJk35No=',
         b'78BQJHX3w1Nayqoe05Af5IMfxpbQNjpPPYcpKUsSYus=',
         b'v9sGcgXR+x8gzRr4dVQXUjVNWsAsgG4VeOaRuaiJ9CU='
         ]
```

It looks more... base64 than rot13. There are symbols in each element of the table, and it ends with an equals sign.

Rot13 does not rotate symbols and I doubt that the plaintext had such weird symbols in between and at the end too.

Hence we hash out the rot13 line in the string, and replace it with a base64 decode function.

Also we suspected that the AES mode was ECB earlier so we make the respective changes as well.

In the end, we get:

```py
import base64
import hashlib
from Crypto.Cipher import AES
import codecs

table = [b'J+TRKJnPo0EAaLH5oio7qW6lC92FUFY96kZPBFqj4NU=',
	b'UERMnI8D1rOakgIExgaBrSqxzgWkSl/PY58t5MuryL4=',
	b'CFXKWz5CUJJyPrQB1Axqo4K7xMEr6xKIxpCz9Q1+Lc8=',
	b'mSzAc6NvQHOiZyCcfa7m/E+CIuDAJHNrYdHqCWO9qs8=',
	b'r/81JVwz7QtfBjZoYoxhx1PFbc+32WmzAfEiROMfs+4=',
	b'mFcLrpE/mJu4kfrgIMjnAhzrYHjyjyUZCOrRbBXjzFw=',
	b'QZV3lswJ5cjD0vPOJD2RNsGmpmJfEidHHdyECpx/AGk=',
	b'H2wx879JqZVmousKnHip3iSMw6MvxeMqgxIY3ZseToY=',
	b'OUUHem3lwgSA2PqLDkp7XY8inlI1fYw0l0wYKVBJ7uA=',
	b'NTAl0vlCxoxlwZCddxSJS5c80ALp0I4DMPwJCb1qer4=',
	b'59meCW3BGFMrVoKS40Net4bogld8uNDLpWmouuTu8iU=',
	b'3dX4EHGlVY8FXEVS4G1KvKhhsnunCoPgmHw9raybrsw=',
	b'3fxahhfCY27tjTduqeW+f8qxtnRCQkabDzzwdixqFUc=',
	b'xbAbq0HnibOd2uKAvmazFvq+5SWzyvKG1gIB/uUqLu4=',
	b'hiw6A01RyDcOJTVSJ1/XxIQLZI40tgX5cOHlFsVFwTM=',
	b'Rq+xfITYIhhdsUpchEBJ7Dinn9wi42+mtjuQhWSY3YM=',
	b'BpUeSFVAnsoFwDiBfVuvRGOe3UKJrnl3Z2wlQ2WDigw=',
	b'+O1srpp/thvcvm7nhdbn4M6Xg0DiVCuxTezRtPX6M6A=',
	b'7BTnOYJA3NyCE+SGGeH6mq+WXbFM+QLd4DmIqVYhZ1w=',
	b'EynIrUqnKQX0rv9cqgEhwkxxUwCb2GLUgN4pTpmKESM=',
	b'GGDLLuW08z4YaXVke4FGf9CcsGa0x+fbclto0ZOT+7o=',
	b'er2/8Q4XeF05y3q4DtgxrC5tfpawfZR5qg55qlcLPA8=',
	b'VptI/j/Vnyrh/H8DH/lyRjgZqIvG1vxXvhV3xJk35No=',
	b'78BQJHX3w1Nayqoe05Af5IMfxpbQNjpPPYcpKUsSYus=',
	b'v9sGcgXR+x8gzRr4dVQXUjVNWsAsgG4VeOaRuaiJ9CU='
]

count = 0

key =  b'i want 16 bytezzi want 16 bytezz'
cipher = AES.new(key, AES.MODE_ECB)

while count < 25:
    string1 = table[count]
    count = count + 1
    #rotten = codecs.encode(str(string1), 'rot-13')
    rotten = base64.b64decode(string1)
    for i in range(999):
       rotten = cipher.decrypt(rotten)
    print(rotten)
```

and our output is

```
b'Tickingawaythemomentsthatmakeupa'
b'dulldayFritterandwastethehoursin'
b'anoffhandwayKickingaroundonapiec'
b'eofgroundinyourhometownWaitingfo'
b'rsomeoneorsomethingtoshowyouthew'
b'ayTiredoflyinginthesunshinestayi'
b'nghometowatchtherainYouareyounga'
b'ndlifeislongandthereistimetokill'
b'todayAndthenonedayyoufindtenyear'
b'shavegotbehindyouNoonetoldyouwhe'
b'ntorunyoumissedthestartinggunAnd'
b'LNC{pythoncryptodomeprogramming}'
b'unbutitssinkingRacingaroundtocom'
b'eupbehindyouagainThesunisthesame'
b'inarelativewaybutyoureolderShort'
b'erofbreathandonedayclosertodeath'
b'Everyyearisgettingshorterneverse'
b'emtofindthetimePlansthateitherco'
b'metonaughtorhalfapageofscribbled'
b'linesHangingoninquietdesperation'
b'istheEnglishwayThetimeisgonethes'
b'ongisoverthoughtIdsomethingmoret'
b'osayHomehomeagainIliketobeherewh'
b'enIcanWhenIcomehomecoldandtiredI'
b'tsgoodtowarmmybonesbesidethefire'

Process finished with exit code 0
```

There is our flag.

```
LNC{pythoncryptodomeprogramming}
```

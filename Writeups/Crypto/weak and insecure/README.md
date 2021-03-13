# weak and insecure
---

### Challenge Description

e = 65537 N = 21805564595370162889 cipher = 10421600892944111639

find the message :) the message should be a 10-digit long number remember to encapsulate the answer in the flag format LNC{xxxxxxxxxx}

---

### Solution

This one was honestly easy, if you were stuck I'd suggest you read up on [RSA Wikipedia](https://en.wikipedia.org/wiki/RSA_(cryptosystem)) and you can do the RSA practices at picoctf, which covers many aspects of vulnerabilities in RSA.

Anyways we are given with e N and c.

To get our message, m = c^d mod N.

We do not have d yet.

You can get d with d = pow(e, -1, phi)

since N=pq and phi=(p-1)(q-1)

We can use factordb.com to get the p and q from N, and then plug it into phi=(p-1) * (q-1) to get phi.

with phi, you can find d and with d you can find m.

```py
e = 65537 
N = 21805564595370162889
c = 10421600892944111639
p = 2625556849 # from factordb.com
q = 8305119961 # from factordb.com

phi = (p-1) * (q-1)
d = pow(e, -1, phi)
m = pow(c, d, N)
print(m)
```

We get the 10 digit number and that is our flag.

```
LNC{5342585545}
```

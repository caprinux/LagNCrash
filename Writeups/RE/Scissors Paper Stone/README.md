#Scissors Paper Stone

---

### Challenge Description

I can't seem to win the computer in scissors paper stone. Can you help me win it?

[scissors.zip](https://github.com/caprinux/LagNCrash/files/6128301/scissors.zip)

---

### Solution

We run the APK in Android Studio and we see that the computer will always pick the choice that will win us. 

The idea is that we want to reverse the choices such that we are the one who will always win rather than the computer.

Looking at the decompiled scissors.apk in Android Studio and following the relevant files, we arrive at 

Java > com.example > scissorspaper stone

I notice a few interesting files named MainActivity$1 MainActivity$2 and MainActivity3.

Since all of these is in smali, which I do not really understand, I look for interchangeable variables in the file. 

In MainActivity$1, I see 

```smali
.method public onClick(Landroid/view/View;)V
    .registers 4
    .line 26
    iget-object p1, p0, Lcom/example/scissorspaperstone/MainActivity$1;->this$0:Lcom/example/scissorspaperstone/MainActivity;

    const/4 v0, 0x1

    const/4 v1, 0x3

invoke-static {p1, v0, v1}, Lcom/example/scissorspaperstone/MainActivity;->access$000(Lcom/example/scissorspaperstone/MainActivity;II)V
    return-void
.end method
```
0x1 and 0x3 seems interesting. Let's look at MainActivity$2 as well. 

```smali
.method public onClick(Landroid/view/View;)V
    .registers 4
    .line 32
    iget-object p1, p0, Lcom/example/scissorspaperstone/MainActivity$2;->this$0:Lcom/example/scissorspaperstone/MainActivity;

    const/4 v0, 0x2

    const/4 v1, 0x1

    invoke-static {p1, v0, v1}, Lcom/example/scissorspaperstone/MainActivity;->access$000(Lcom/example/scissorspaperstone/MainActivity;II)V
    return-void
.end method
```

It seems that there are constants 0x1 0x2 and 0x3. Perhaps that might be the scissors paper stone but in smali.

I go to my linux and decompile my apk with apktool 

```
$ apktool d scissors.apk
```

I go to the MainActviity part of the code in my decompiled scissors.apk folder, and flip the values of the constants around for MainActivity$1,$2 and $3.

Then, I rebuild my apk with

```
$ apktool b scissors
```

and sign my apk with 

```
$ ./uber-apk-signer-1.2.1.jar -a scissors.apk
```

and then run the app in my emulator once again.

Voila! I won. 

```
YOU CHEATER
CHEATERRR!
LNC{y0u_cH34tEd}
```

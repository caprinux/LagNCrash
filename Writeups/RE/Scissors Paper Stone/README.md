#Scissors Paper Stone

---

### Challenge Description

I can't seem to win the computer in scissors paper stone. Can you help me win it?

[scissors.zip](https://github.com/caprinux/LagNCrash/files/6128301/scissors.zip)

---

### Solution

_p.s. i'm quite bad at reading java and smali so this writeup may not be so good for ur eyes but welp_

We run the APK in Android Studio and we see that the computer will always pick the choice that will win us. 

The idea is that we want to reverse the choices such that we are the one who will always win rather than the computer.

First we look at the decompiled apk in [jadx](https://github.com/skylot/jadx)

If we go to MainActivity, we see this:

```java
            public void onClick(View view) {
                MainActivity.this.reeeee(1, 3);
            }
        });
        this.paper.setOnClickListener(new View.OnClickListener() {
            /* class com.example.scissorspaperstone.MainActivity.AnonymousClass2 */

            @Override // android.view.View.OnClickListener
            public void onClick(View view) {
                MainActivity.this.reeeee(2, 1);
            }
        });
        this.stone.setOnClickListener(new View.OnClickListener() {
            /* class com.example.scissorspaperstone.MainActivity.AnonymousClass3 */

            @Override // android.view.View.OnClickListener
            public void onClick(View view) {
                MainActivity.this.reeeee(3, 2);
            }
        });
    }
```

We see that there are some variables 1, 2 and 3 being loaded into MainActivity.

Though I'm not good at reading Java, I'm pretty sure that these are the lines that decide the computer's moves based on our moves.

i.e. if we do 1, computer will do 3 and the computer will always win

Hence we want to reverse this and we will have to decompile the apk then edit the smali code.

However, let's first have a look at the smali code. 

I go to my linux and decompile my apk with apktool 

```
$ apktool d scissors.apk
```

Now we move to the directory where MainActivity is, scissors > smali > com > example > scissorspaperstone

We see some MainActivity files; MainActivity$1,$2,$3 and MainActivity.smali

MainActivity$1,$2,$3 looks interesting for our case

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
0x1 and 0x3 seems interesting. It looks like the same constants that we saw in Java. 

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

Yup it definitely is. Let's now use a text editor, vim/nano/**leafpad**, to swap the constants around.

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

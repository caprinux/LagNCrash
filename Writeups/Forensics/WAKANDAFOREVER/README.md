# WAKANDAFOREVER

---

### Challenge Description 

Black Panther left you a message? If you are a true Wakandan you would be able to read it! WAKANDA FOREVER!

Capitalize the LNC in the flag you get

[WAKANDAFOREVER.zip](https://github.com/caprinux/LagNCrash/files/6134334/WAKANDAFOREVER.zip)

---

### Solutions

There is a file attached to this challenge with no obvious file type. 

Checking the file type in linux gives us

```
$ file WAKANDAFOREVER 
WAKANDAFOREVER: data
```

Hmm that's not too useful. If you try to concatenate the file, it outputs a ton of gibberish. 

Let's look at this file in hexdump and see the file header.

![image](https://user-images.githubusercontent.com/76640319/111022946-0d74c900-8411-11eb-926b-b3374b4fde67.png)

From here, we can see that this file is an AD image, which we can open with FTK Imager.

![image](https://user-images.githubusercontent.com/76640319/111023565-c983c300-8414-11eb-9bad-cdd7a20c6c96.png)

We then go on to export all the files into a folder and we look through the 101 files.

There are a bunch of [.FILESLACK](https://www.canto.com/blog/file-slack/), which can be ignored.

We go on and we see a bunch of pictures of baby yoda:

![download(10 - Copy (11)](https://user-images.githubusercontent.com/76640319/111023636-1a93b700-8415-11eb-9202-da72dddd918f.jpg)
![download](https://user-images.githubusercontent.com/76640319/111023637-1b2c4d80-8415-11eb-9450-c77123bf1477.jpg)

Okay but that's not helpful either. We then go on to see a bunch of empty txt files. However, something catches my eye.

There are 74 empty text files which are basically a copy of UserData. 

However, there is 1 file called ``userdata.txt`` which is 4kb, and if we open this in notepad, we can see a PNG header.

```
‰PNG

   
IHDR  Ã   0   m÷Ý   sRGB ®Îé   gAMA  ±üa   	pHYs  Ã  ÃÇo¨d  µIDATx^íÝ=xÚÖðïÐ²™éŠåVž.w¹f
,
^je	d1,–š,,.–'ã%ÂK Kpã»€—@Ë¤‹è]Jº NÐIÙ”,½œ`#dÀÂ¼¿ç9±¬Ä	ÑÇyÏ÷ùB÷þoB!+ìÆWB!deQ0$„²ò(BYy	!„¬<
†„BVCB!+‚á¢i¿C~ùÑíï«iÆIB!–hgv¹c=O¥y†£AÉí Vâ°Ÿ?@Øã4ÎB¹)½[C!±ƒ¼s•lî)³X
†ÑA)r15‰Úùð9ŒÓ„BfHC%ñ_„›1ÈÕø§ˆçN‡"}‹MQEJþ™MŠ„„27Ú+Ä¹ŠÂÚÕxãô$Ôg8oÍ—H‹
vC€!!„Ì—Ó‡­ û*ï UêÏ™@ÁpÎYB?kƒ_	!„,Fµ C5Ž'¡`8W]¨JÏ8n@5_H!„bI½‹XWÐ69ÀÔB0T }õ%S¦ÐÕÕô&¤ûýŸÎþUFy>ø»üRúàÌH·„ÕÏx­‘›‚
Zf‹(ŸÑÑ>þüÈõ¼œ¶aå£wKÛc~¶‹Ò6û;·Kìh>š•’éÒÚuÿù/ÿÝãÿý2žÅñ÷y˜Ì>§·ugmf×ÙÈ?¤Af2Ú¦ïO¹{ì>ÌíÃÜÐg×ÊxnŒVîÉÂžÇn–qÌò`Íd°°9x²YHfS*„
ã'§¥–v±7lc¼¤!î¢Ü1¾±5ÜHï­ÜhGÓÐ H!ÔaG°¿t}w„ÁZ*z;d6ñÎqðŽÜßKéïY §<Çò'õ¢5èº•eØ~ŽýÀRQ88m8Bo×Q6Ž7x—q8…`¸!ùIÓé‘éÑ<—°R–$61îYá¸&Äìvç‚Û÷ñ?Ðjw§üÌÈ‰‡ËJÕCôû„á]º¾Qÿà._¨¡Ábàš3­âßN¢#÷÷r²ø.‘qÃåÐ+ÕX½ïzº"³{ÏºqfJš‚bîŠÊ¢rÁÔªqø5N—î©#Wßïf¹°9‚!Ë ÙMÈMLÇ°~ŸtÔ÷wpä=„”4NHdá-ì"«Ø?'å7‚ÆÓ}k¾yWÿÅÈ}ÄµÇ«?Àw—æè¯?B¡qŽ¨–B0ðKp	1Á8–„bíº‡ºƒJNBÏƒßcœš–®¢’J¡¢ÞÎËóÎøjGÝ^Ý8b'Á3Ï©=4ÙMHOLX¾OÍ—ó<¤½ðøÿf5C²©—K`·ÍéöaêÊ+õI-œG¨ŸÄà¾‹32œ›ÈT_#¡‹ØÜbÑþÕ|rºJ	ÅJ¿O¤ƒj±efKsâ‰!òÑïQº¢+§ßý“®rHdÃj,¼m]µbÙ[`Ã|[Š…`èCúÝ{è×¤Nq¤64µŠb
jâ ñkžOü 	5…Ì{ÕçlÝ
¯qÈùxSUv¥°sá•üƒ»Ý,ædÏˆû`1ðòRô“ÅÐPßýëþöÊýÑ-”Å6ùo8³óƒâ„=Å>_FLØF®ÖýØ¤wQ—Â-ƒÏœÒsâÞ`¥‘¼Ë|“š…`8_Ú™QBJoâÚGÅ±‰T6ˆjJB})šÙdB>ãøz.ágTÒ>öZ-‚‚Lô[[)‚Ì˜AKsÓˆç, :*
ßcP	˜Ê-~Û[Îë£GXPiÀ?Ò²»ìx8
&P´s<tôyçHñ
ÒÂ×p£€k_CÛðæ~¼ß«
çÿ<:7ÃH
†jpS]c{C]AV,€Ïî!l¢
å
ïAâsHåó}q¦+5Èì«7{€¨É¾rÞãYà‹ÒÃ›zõ‘ôÆÄ`¸™d?CrËû¿»²yi<|~[[Âë£½B&.ƒ ˜ãbM{§'Œ|ñ!NF<óÊò ºzqÜ‡«’Å±ýn€óßÐ–O°êÞ@(s‚šúÊ=*èÚÑžGVQËxÑ‚Ü4Ÿ™Ø*6Ïí%Ù‹`¶%ÝÃ^Š$zâ¾=§Zh
r©œ™×“öëp…O?kâÖßý‰ãÁ°Õsx,Ÿ£à¯#æˆœ‰a¦¶úü3Õ¢Üü^Öë£)2Š,x¤ÒcÆ¬³Rš—¢Œ†Åhx^7Æáªtƒ±,äñ¬–
öKÁëDÃð™ÚhAð3ì§Yäóèðý€b9Š7ñ„©¼¤Ï>Á°SB&Õ@ ›†Šj‘c3
) Ûlª…†öÙD£/áÌþye›D¦äø¢'¯q,´‘–í>4j^ú»ï`Ó¸¢û]¾}Û¯*lÂã~ÿ)÷Æ&ûµmÚËÃ2ÈÏ3ãIéÔT+ÕmQ+¹1µÙEN¹°/>øš­Tq‰ceb¡Æ>Á•øÊìá+Oýä¹>}5ÿÀMZå{8¾ú'R-¤êOˆß©9‹à„Ë‘›/àÿê_H];D~]tïKðÏj›îð%°ïm 8ÅºŠd5µÊÒ˜ÚìíM¹°‡;Œlõ"WA|½—ÒÕ…lÛ
 YvNVÑßý…ì†‚äýáh2bÖp‘¡è„$Ür³²ç1¤‡¼8Ýôµ´½^¹IÀ®3èCýÍ“+¦ÝqkkýÂP
ÍöðûOµ[5ö+ç´åLc‰±éÒŒ—Û›±@ñÏOj²Ã4}¥â2]¿#ïÝr‘md{A¿e•¦ôÕùÊŠCc¼Ox>‚þâ
Õè×Ÿÿž¥—Á	÷ƒ§(WÓÐÅ{$ÅFM¸v5Ü 9XáQ8?EÒsÛ
Ë¬†?ÀÎ5Ów\üÅ¤ƒÎ^<QÆVæ Á›6­±œ~UÛœ>Q´•Æ,Ù×)±ó- *À;íâ<ã—¹»"¥_V“öqQå¥¥ž=ßûÎ½Sdc>¸&d)«Y3Ü!5úàk>n„Ò³}œ>$³Ih"Ëäs«Úf‚±ÚN¬!à¸þ?Dí²Ê€óÒƒé;9SÓwš…]ä¹Ä°Å%¶ÈóÄ‚€^9‚h¢„¦Q’Ôš%$¢”{
â£éëÆ,qwuJ¾e¤+Ï
°Æ
¦fó“Õ†ü#$.=øÃ5ù`räÜl^‡oýµ´©]{Ïº-z¹Ðâjù'Ø-Žðƒé;¤Ió=ºý`*Ù§ðÙ$–/3>–G%îE£¶Ò¸¼5¼Ø©äMOS"+H¯!Ñ`UaŠµ_-ÃÉ[8­G—c©žÅ’!–mº]Ëmé/;·ui-¹ú#[fpÃé;-Q‚|e[·¥øÕÀ!R´ªÈŒ¬CÈÿ‚NýÓyz¿ ÿ€"ábTã/òõûØ3ÎÚV+!;èòìM5ˆÍB04»…S<å@§mìtÏn¢.Írs§Õ î#ïÈ v~ ¿;ÈúÓwrþ
ÄB}ÌÅôê¬V(:ké.Õ4²%àò-ó<½ù›×Ô
“§³´aƒ‘Þ´[ã¨µkþ:X†f·pŠ-ÏN.«Ó:&ÓÚ
>®¡N:gH¶PtfQéïÈaûB“ÁÌ8ñÄÏ–Žj!›ÚE/uxíZº„ÌÃ¼¦V8}—·Šû–¬V^m]1$yÁÐìNéÕJÏ•R[#MÆ®5š|Ï(å ªüäòSÜú Q³ÖÃ3zcFœ÷D†V ‹dàÇM«è§›ì5åxL^þ1û%üÀº-§þ¸¸ûõä¶éøc!šÝÂÉHÙ2+;¯ª.ÚÊÇÜÓïu¯ìpùQ÷ÉnMÕŸˆÄ˜V¢­LÝXr‡èje|~n¤£þ‚Ë6Åñ#M4õ¶é®©/X)âoã˜Ì\¹{÷6J©ú{dÌm\A!ÄŠî+DùÊƒo‚8VÍ-©g¡fHÌãX)Å8d7eŠ}&	!„XáZ‡÷ÃqN“-PçÊÞwÑ®æO#á!dÎF*!‚n“]çÌ'¤î{xGkçBÈÂDãAÓƒ|(ÎÛ`Áç~8,AV(BÈ\i
Î«¬~:4?²Ð,D•-„ÛqÔÎ—a^!„,£á‚ÿ159u^K5Ã…XGðèW(Ñb[OPºXy˜BÈLèýíš¶8âÑ´Pé šá¢i¿C®P®t±‘üÉMš Fì®¿ÐFæ×ò0áßÁ¥[Í„Ø”vi§€7¾0v¢Aø&íÕt
†„	ú‹óÏx¡æÌkèištKìƒ‚!!„•G}†„BVCB!+‚!!„•GÁBÈŠþÏçb5Ò
    IEND®B`‚
```

This is probably a png file.

Now we go back to our linux and we display the picture:

```
$ display userdata.txt
```

![image](https://user-images.githubusercontent.com/76640319/111023699-8413c580-8415-11eb-8383-d07f2d3be75b.png)

These are Wakanda Alphabets.

If you google Wakanda Alphabets, you will find this:

![image](https://user-images.githubusercontent.com/76640319/111023731-aad1fc00-8415-11eb-81f5-87b533698ff2.png)

Decoding this gives the flag:

```
LNC{Wakanda_for3va}
```




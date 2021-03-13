
# The Oracle
---

### Challenge Description

I found this old oracle scroll, What is it trying to tell me?

[yingyang.zip](https://github.com/caprinux/LagNCrash/files/6134193/yingyang.zip)

---

### Solution

We are given a text file with many Yin Yang and Oracles in the text.

Being the smart alecks I was, I tried to remove all the Yin Yang and Oracle with my trusty NotePad replace function and found out that there were some anomalies.

```racleYanYn```

There were 3 words that were mispelt. 

.

.

.

.

.

Okay nvm that was just a distraction. We literally gave up on the challenge when we couldn't figure out what was racleYanYn. 

Anyways we come back the following day after some team was flexing on us how easy Oracle is.

After a few hard minutes of staring at the screen, I noticed how Oracle only appear once in a few Yin and Yangs, while Yin and Yangs are just continuously appearing.

Then it hit me OHHHHHHHHHHHHH WHAT IF THIS IS BINARY

But first, we had to fix the typos racleYanYn so that we can convert it properly.

Then we changed Yin to 0, Yang to 1 and Oracle to space.

We plug it into cyberchef and decode from binary

```
In Ancient Chinese philosophy, yin and yang (/jɪn/ and /jɑːŋ, jæŋ/; Chinese: 陰陽 yīnyáng, lit. "dark-bright", "negative-positive") is a concept of dualism, describing how seemingly opposite or contrary forces may actually be complementary, interconnected, and interdependent in the natural world, and how they may give rise to each other as they interrelate to one another.[1] In Chinese cosmology, the universe creates itself out of a primary chaos of material energy, organized into the cycles of Yin and Yang and formed into objects and lives. Yin is the receptive and Yang the active principle, seen in all forms of change and difference such as the annual cycle (winter and summer), the landscape (north-facing shade and south-facing brightness), sexual coupling (female and male), the formation of both women and men as characters and sociopolitical history (disorder and order).[2]

There are various dynamics in Chinese cosmology. In the cosmology pertaining to Yin and Yang, the material energy, which this universe has created itself out of, is also referred to as qi. It is believed that the organization of qi in this cosmology of Yin and Yang has formed many things.[3] Included among these forms are humans. This is the flag DualityW1thTh3OrAcle. Many natural dualities (such as light and dark, fire and water, expanding and contracting) are thought of as physical manifestations of the duality symbolized by yin and yang. This duality lies at the origins of many branches of classical Chinese science and philosophy, as well as being a primary guideline of traditional Chinese medicine,[4] and a central principle of different forms of Chinese martial arts and exercise, such as baguazhang, taijiquan (t'ai chi), and qigong (Chi Kung), as well as appearing in the pages of the I Ching.

The notion of duality can be found in many areas, such as Communities of Practice. The term "dualistic-monism" or dialectical monism has been coined in an attempt to express this fruitful paradox of simultaneous unity and duality. Yin and yang can be thought of as complementary (rather than opposing) forces that interact to form a dynamic system in which the whole is greater than the assembled parts.[5]  According to this philosophy, everything has both yin and yang aspects (for instance, shadow cannot exist without light). Either of the two major aspects may manifest more strongly in a particular object, depending on the criterion of the observation. The yin yang (i.e. taijitu symbol) shows a balance between two opposites with a portion of the opposite element in each section.

In Taoist metaphysics, distinctions between good and bad, along with other dichotomous moral judgments, are perceptual, not real; so, the duality of yin and yang is an indivisible whole. In the ethics of Confucianism on the other hand, most notably in the philosophy of Dong Zhongshu (c. 2nd century BC), a moral dimension is attached to the idea of yin and yang.[6]
```

There is our flag.

```
LNC{DualityW1thTh3OrAcle}
```

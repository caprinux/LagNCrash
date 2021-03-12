# Hungry Go Where?
---

### Challenge Description

My eyesight isn’t very good… can you help give me some food?

http://challenge2.lagncrash.com:23887/

---

### Solution

Opening the source code of the website, we find something interesting

```html
        <div>
            <a id="flag" style="display: none;" href="canyouseeme.txt" download>Here you go! I found this piece of paper but it looks kinda weird though...</a>
        </div>
```

There is a file called canyouseeme.txt. 

If you append /canyouseeme.txt to the end of the website you can view the file. However 

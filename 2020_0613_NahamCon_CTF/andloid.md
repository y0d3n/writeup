# Andloid

## Candroid 50pt

```txt
I think I can, I think I can!

Download the file below.
```

[candroid.apk](./attachments/candroid.apk)が渡される。
apktoolで展開してgrepでおわり

```txt
$ apktool d candroid.apk
$ grep -rwn flag
(snip)
res/values/strings.xml:31:    <string name="flag">flag{4ndr0id_1s_3asy}</string>
```

## Simple App 50pt

```txt
Here's a simple Android app. Can you get the flag?

Download the file below.
```

[simple-app.apk](./attachments/simple-app.apk)が渡される。
Candroidと同じ方法で解けちゃった。想定外かな

```txt
smali/com/example/simple_app/MainActivity.smali:50:    const-string v0, "flag{3asY_4ndr0id_r3vers1ng}"
```

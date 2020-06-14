# Misc

## Read The Rules 5pt

```txt
Please follow the rules for this CTF!

Connect here:
https://ctf.nahamcon.com/rules
```

ルールのとこのソースに書いてる

```txt
flag{we_hope_you_enjoy_the_game}
```

## CLIsay 20pt

```txt
cowsay is hiding something from us!

Download the file below.
```

`clisay`というELFバイナリが渡される。
stringsででてくる

```txt
$ strings clisay
(snip)
flag{Y0u_c4n_
  __________________________________
/ Sorry, I'm not allow to reveal any \
\ secrets...                         /
  ----------------------------------
         \   ^__^
          \  (oo)\_______
             (__)\       )\/\
                 ||----w |
                 ||     ||
r3Ad_M1nd5}
(snip)
```

## Metameme 25pt

```txt
Hacker memes. So meta.

Download the file below.
```

`hackmeme.jpg`が渡される。
stringsででてくる

```txt
$ strings hackmeme.jpg
(snip)
    <rdf:li>flag{N0t_7h3_4cTuaL_Cr3At0r}</rdf:li>
(snip)
```

## Mr. Robot 25pt

```txt
Elliot needs your help. You know what to do.

Connect here:
http://jh2i.com:50032
```

wgetしてgrep。画像に埋め込まれてたっぽい

```txt
$ wget -q -r http://jh2i.com:50032/; grep -r -Poi flag{.*}
jh2i.com:50032/robots.txt:flag{welcome_to_robots.txt}
Binary file hackermeme.jpg matches
```

## UGGC 30pt

```txt
Become the admin!

Connect here:
http://jh2i.com:50018
```

ログインページ。試しに`username`に`yoden`と入力すると

```txt
You are logged in as yoden.
Sorry, only admin can see the flag.
```

`admin`でログインすればいいっぽい。

`Cookie: usr`を見ると`lbqra`という値がセットされている。`yoden`のrot13だ。  
`admin`のrot13で`nqzva`を`Cookie: user`にセットするとflag。

```txt
You are logged in as admin.
Congratulations here is your flag!

flag{H4cK_aLL_7H3_C0okI3s}
```

## Pang 40pt

```txt
This file does not open!

Download the file below.
```

[pang](./attachments/pang)というファイルが渡される。  
中身見ると`臼NG`から始まるので拡張子を`.png`にして開けばflag。

```txt
flag{wham_bam_thank_you_for_the_flag_maam}
```

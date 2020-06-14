# misc

## Fake File 100pt

```txt
Wait... where is the flag?

Connect here:
nc jh2i.com 50026
```

ncしてそのまま`ls -al`したら以下。

```txt
user@host:/home/user$ ls -al
ls -al
total 12
dr-xr-xr-x 1 nobody nogroup 4096 Jun 12 17:08 .
drwxr-xr-x 1 user   user    4096 Jun  4 18:54 ..
-rw-r--r-- 1 user   user      52 Jun 12 17:08 ..
```

`..`という名前のファイルがあるようだが、`../`と名前が被っているので`cat`とかできない。

カレントディレクトリ配下のファイルでgrepしたらflag。

```txt
user@host:/home/user$ grep -rwn flag{
grep -rwn flag
.. :1:flag{we_should_have_been_worried_about_u2k_not_y2k}
```

## Alkatraz 100pt

```txt
We are so restricted here in Alkatraz. Can you help us break out?

Connect here:
nc jh2i.com 50024
```

`ls`すると`flag.txt`があるが、`cat`とかが`not found`になる。  
`less`,`more`,`vim`,`grep`なども試したが、`not found`。

適当に試してたら`echo`は使えた。

`echo ファイル内容 出力`とかでググったらechoだけで内容を出力する方法がでる。

```txt
echo $(<flag.txt)
flag{congrats_you_just_escaped_alkatraz}
```

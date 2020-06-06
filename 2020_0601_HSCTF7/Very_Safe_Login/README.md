# Very Safe Login

100pt (736 Solves)

```txt
Bet you can't log in.

https://very-safe-login.web.hsctf.com/very-safe-login

Author: Madeleine
```

## 概要

![Very Safe Login](../img/Very_Safe_Login.png)

ログインページ。

ページ開いてすぐに脳死で`"`とか`'`とか入力したけどなんも起きない。

## 解

ソースコードみてると普通に書いてあった。

```js

var login = document.login;

function submit() {
    const username = login.username.value;
    const password = login.password.value;

    if(username === "jiminy_cricket" && password =    "mushu500") {
        showFlag();
        return false;
    }
    return false;
}

```

Username: `jiminy_cricket`, Password: `mushu500`でログインすればflag。

```txt
flag{cl13nt_51de_5uck5_135313531}
```

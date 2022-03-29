# HTTPS利用の手引き
以下、HTTPSを用いたサービスを新たに提供したい場合の注意事項についてまとめたものです。

## 1. リバースプロキシについて
リバースプロキシはクライアントとサーバーの通信の間に入り、  
サーバーの応答を代理しつつ、中継する機能及びサーバーの総称です。

クライアントから見てサーバーとして
サーバーから見てクライアントとして振舞うことで様々な問題を解決できます。

クライアント <--> リバースプロキシ <--> サーバー

本環境ではDockerと組み合わせて利用することで、HTTP通信を誰でも比較的簡単に利用することができます。



## 2. 環境変数

### 1. 環境変数の意味

### 2. 本環境におけるルール

## 3. 例
nginxがホスティングするwebサーバーの例です。
ディレクトリ構造は以下のようになっています。
```
.
├── HP
│   └── dist
│       └── index.html
├── docker-compose.yml
└── nginx
    ├── default.conf
    └── nginx.conf
```

```yml
#docker-compose.yml

MyHP:
    container_name: MyHomePage
    image: nginx:latest
    restart: always
    ports:
        - "8080:80"
    environment:
        - VIRTUAL_HOST=myhp.nitkpc.com
        - VIRTUAL_PORT=8080
        - LETSENCRYPT_HOST=my.hpnitkpc.com
    volumes:
        - ./HP/dist:/var/www/html
        - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
        - ./nginx/nginx.conf:/etc/nginx/conf.d/server.conf
```

# HTTPS利用の手引き
以下、HTTPSを用いたサービスを新たに提供したい場合の注意事項についてまとめたものです。

## 1. リバースプロキシについて
リバースプロキシはクライアントとサーバーの通信の間に入り、  
サーバーの応答を代理しつつ、中継する機能及びサーバーの総称です。

クライアントから見てサーバーとして
サーバーから見てクライアントとして振舞うことで様々な問題を解決できます。

クライアント <--> リバースプロキシ <--> サーバー

本環境ではDockerと組み合わせて利用することで、HTTP通信を誰でも比較的簡単に利用することができます。


## 2. 本環境における規則とガイド
## 1. サブドメイン
ドメインに関する基本的な説明は[こちら](https://www.nic.ad.jp/ja/dom/system.html)

全てのサービスには１つのサブドメインが付与されます。
2LDはプロ研HPに使用されるため、3LD以降を利用します。

1. 2LDは使用できません 
2. 3LDの取得数は最大nつです
3. *.nitkpc.com以外のドメインを登録することは禁止します

## 2. HTTP
本環境ではHTTPSによる通信のみが可能ですが、
原則的にプロキシとサービス間の通信はHTTPで行われるため、
特段気にかける必要はありません。
仕様は以下の通りです。
> HTTPで着信された通信は全てHTTPSへリダイレクトされ、
> [HSTS](https://developer.mozilla.org/ja/docs/Web/HTTP/Headers/Strict-Transport-Security)が有効である旨を通知します。
> max-ageは31536000秒(1年)です。

## 3. 具体的な構築ガイド

### 1. 環境変数
プロキシはdocker起動時または、
docker-composeで定義された環境変数に基づいて
SSL証明書とプロキシ設定を更新します。
#### 1. 必須
この項目に示す環境変数は必ず指定してください。
システムの正常動作、管理に必要です。
1. 

### 2. docker

### 3. docker-compose


## 4. 例
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
        - "80"
    environment:
        - VIRTUAL_HOST=myhp.nitkpc.com
        - VIRTUAL_PORT=80
        - LETSENCRYPT_HOST=myhp.nitkpc.com
    volumes:
        - ./HP/dist:/var/www/html
        - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
        - ./nginx/nginx.conf:/etc/nginx/conf.d/server.conf
```

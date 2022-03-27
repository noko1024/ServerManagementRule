# 新規ユーザの追加
#### 作成日：2022.03.27
以下、sudo権限所持者が新たにユーザーを登録する際の注意事項をまとめたものです。

## 1.グループについて
ユーザーを新規作成する際は、必ず**member**グループに所属させるようにしてください。
**member**グループに所属していないユーザーは、公開鍵認証方式でのみssh接続を許可する設定にしています。
``` 
# /etc/ssh/sshd_config

PasswordAuthentication no
PubkeyAuthentication yes

Match Group member
PasswordAuthentication yes
PubkeyAuthentication no
```

## 2.ユーザーの追加
前項の注意事項を踏まえ、ユーザーを新規登録をする際は以下のコマンドを入力してください。
```
$ sudo adduser --ingroup member ユーザー名(githubのユーザー名が好ましい)
```
また、パスワードの設定後に出てくる項目については、
```
Changing the user information for ...
Enter the new value, or press ENTER for the default
    Full Name []: 本名(英字で)
    Room Number []: 
    Work Phone []: 任意回答
    Home Phone []: 
    Other []: ユーザー名 is 役職名(英字で) in 2022 
Is the information correct? [Y/n] y
```
と入力してください。ただし、役職名に関しては以下の語群に記載されているものにしてください。
```
会長：　president
副会長：vice-president
会計：　accounting-clerk
監査：　auditor
その他：member
```

## 3.sudo権限の付与について
セキュリティの観点から、被付与ユーザーが**公開鍵認証方式でssh接続をする設定に変更している場合のみ**、そのユーザーに対してsudo権限を付与することを許可します。<br>
[公開鍵暗号化方式の設定の手順はこちら](authPubKey.md)
### sudo権限を付与する
```
$ sudo usermod -aG sudo ユーザー名
```
### memberグループから除外する。
```
$ sudo gpasswd -d ユーザー名 member
```

## 4. その他コマンド
その他、ユーザーやグループを管理する際に知っておくと便利なコマンドを載せておきます。詳細を押すと、そのコマンドに関するリファレンスに飛びます。
### 4-1.　パスワードの変更　[詳細](https://atmarkit.itmedia.co.jp/ait/articles/1612/05/news021.html)
```
$ passwd 
```
### 4-2. ユーザーの一覧を表示　[詳細](https://kazmax.zpp.jp/linux_beginner/etc_passwd.html)
```
$ cat /etc/passwd
$ cat /etc/passwd | grep ユーザー名
```
### 4-3. ユーザーを削除する　[詳細](https://atmarkit.itmedia.co.jp/ait/articles/1811/09/news031.html)
```
$ sudo userdel -r ユーザー名
```
### 4-4. グループの作成　[詳細](https://atmarkit.itmedia.co.jp/ait/articles/1811/15/news025.html)
```
$ sudo groupadd グループ名
```
### 4-5. グループの一覧を表示　[詳細](https://kazmax.zpp.jp/linux_beginner/etc_group.html)
```
$ cat /etc/group
$ cat /etc/group | grep グループ名
```
### 4-6. グループの削除　[詳細](https://atmarkit.itmedia.co.jp/ait/articles/1811/16/news043.html)
```
$ groupdel グループ名
```


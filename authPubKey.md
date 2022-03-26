## 公開鍵認証方式とは
#### 作成日：2022.03.27
公開鍵認証方式とは`秘密鍵`と`公開鍵`の2種類を用いて認証を行う方式です。  
IDとPasswordを用いる認証よりも安全性が高いのが特徴です。


## 手順
まずssh-keygenを用いてキーペアを作成します。
```
$ ssh-keygen -t ed25519
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/hoge/.ssh/id_ed25519):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/hoge/.ssh/id_ed25519.
Your public key has been saved in /home/hoge/.ssh/id_ed25519.pub.
The key fingerprint is:
SHA256:pXXxKKeJTZ9ZtWtBi8hHdBhYplRD9xfydPsJuEF6f+k hoge@nitkpc
The key's randomart image is:
+--[ED25519 256]--+
|           oB@o*o|
|          =.*=X.B|
|         .=B++o*o|
|         B.B=+. B|
|        S +.+. *.|
|              +  |
|               E |
|                 |
|                 |
+----[SHA256]-----+
```
すべてEnterで構いません。

正常に鍵が作成されれば、自身のホーム直下に.sshディレクトリが出来ているはずです。
(.sshは隠しファイルであることに注意してください。)  
```
例)
    Windows C:\Users\hoge\.ssh
    Mac     /home/hoge/.ssh
```
以降の作業は.sshディレクトリで行います。

```
$ cd /home/hoge/.ssh
$ ls -1
id_ed25519
id_ed25519.pub
```
.sshディレクトリはこのようなファイル構造になっており、  
id_25519が`秘密鍵`  
id_25519_pubが`公開鍵`です  
`秘密鍵`はユーザーを証明するものです。  
取り扱いには十分注意し、<u>漏洩が疑われる場合はすぐに更新してください。</u>

<br>
次にサーバーに公開鍵を登録します。
クリップボードにコピーしてサーバーのシェルに移動します 

```
$ cat id_25519.pub 
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIGuRKg3SnS+5/Fu7IuxYxcn1p497Y50zluXgrJ+FHne2 hoge@nitkpc

```
一度vimなどでファイルに保存してから、登録してください。
```
//シェルだけで行いたい場合
$ cd ~/.ssh
$ echo "ここに貼り付ける" >> ~/.ssh/id_25519.pub
$ ssh-copy-id -i ~/.ssh/id_ed25519.pub ユーザー名@ssh.nitkpc.com
$ ls -1
authorized_keys
id_ed25519.pub
```
.sshディレクトの中に`authorized_keys`ファイルが生成されていれば正常に完了しています。  
以降は公開鍵認証方式を用いて接続してください。
```
$ ssh -i ~/.ssh/id_ed25519 ユーザー名@ssh.nitkpc.com
```

## configファイルについて
configファイルを記述すると、`-i`や`ユーザー名@`のような情報を指定しなくても接続することができます。  
クライアント側(接続する側)の.sshディレクトリにconfigファイルを生成します。
```
$ cd /home/hoge/.ssh
$ touch config

//Windowsの場合はメモ帳などで作成できます。
//congig.txtなどではなく、"拡張子がない状態"で必ず保存してください。
```
以降はVim等を用いて書き込んでください。  
例えば以下のように設定すると`ssh proken`で接続できます。
```
Host proken
    Hostname ssh.noko1024.net       //サーバーのアドレス
    User ユーザー名                 //ログインするときのユーザー名
    Port 22                         //sshのポート番号
    IdentityFile ~/.ssh/id_25519    //秘密鍵が保存されたファイルのパス
```

## 補足
ssh-keygenでは`-t`オプションを用いてed25519(エドワーズ曲線デジタル署名アルゴリズム)を指定しました。  
ed25519はRSAに対して安全面と性能面の両面から優秀です。  
特段の事情がない限りこちらを用いることを強く推奨します。
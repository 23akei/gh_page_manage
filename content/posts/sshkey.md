---
title: "sshの設定で(個人的に)よく詰まるとこ"
date: 2020-05-06T13:01:29+09:00
draft: false
categories: ["技術","備忘録"]
tags: ["ssh","github"]
---

　最近なにかとssh接続を使う機会が多く、２日連続で同じミスをやらかしたのでちゃんとまとめておく。  
　まとめるけど詳細は書かないのでこれを取っ掛かりにしてググって。これはそういう記事。  
<br>
<br>
<br>
  
#### 接続について  
　sshそのものや、やり方についてはqiitaとかにいっぱい記事があるのでそちらを参照あれ。  
<br>
<br>
<br>
  
#### sshでエラー出たら
　とりあえずsshのデバッグコマンド（オプション `-vT` ）を実行する。
```bash
$ ssh -vT git@github.com
```
　（例としてgithubを接続先にしてある。）  
　オプションの `v` を省略して `-T` とすると単純にテスト接続ができる。
<br>
<br>
<br>
  
#### config  
　`/home/.ssh/config` で秘密鍵の設定したかどうか。
```bash
Host github # 接続名
  User git # 接続ユーザー。githubの場合はgit
  HostName github.com
  IdentityFile ~/.ssh/(秘密鍵のファイル)
```
<br>
<br>
<br>
  
#### permisson  
　秘密鍵ファイルの権限。強すぎても弱すぎてもだめ。  
```bash
$ chmod 600 (秘密鍵ファイル)
```
<br>
<br>
<br>
  
#### ssh-add  
　秘密鍵の追加。コンソールごとに必要なので毎回自動で実行するようにするといい。
~~~bash
eval `ssh-agent` > /dev/null
ssh-add ~/.ssh/(秘密鍵ファイル) >& /dev/null
~~~
<br>
<br>
<br>
  
だいぶ適当なので後日加筆訂正するかも。
  
* * *
###### 主な参考  
https://qiita.com/nyanchu/items/32d65c4c36315b876d38  
https://qastack.jp/superuser/325662/how-to-make-ssh-agent-automatically-add-the-key-on-demand


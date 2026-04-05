## Linux Server Practice

## 概要
Ubuntu環境上でWebサーバーを構築し、HTMLを配信する練習を行いました。
Pythonの簡易HTTPサーバーに加え、Nginxを用いたWebサーバーの公開も経験しました。

## 環境
OS: Ubuntu Server
仮想環境: VirtualBox

## ディレクトリ構成
server-study/
├── config/
│ └── nginx-server-study.conf
├── html/
│ └── index.html
└── logs/

## 実装内容
1. Python HTTP サーバーでの配信
   'python3 -m http.server 8000' を使用してローカルでHTMLを確認
2. NginxによるWebサーバー構築
   ・'/var/www/server-study/' に HTML を配置
   ・Nginx サイト設定ファイル '/etc/nginx/sites-available/server-study' を作成
   ・symbolic link を 'sites-enabled' に作成して有効化
   ・所有者と権限をNginxユーザーに設定
   ・ブラウザで 'http://localhost/' にアクセスし、'Hello Linux Server' の表示
3. GitHubでの管理
   コミット履歴によって、サーバー構築の過程を可視化

## 学んだこと
・Linux環境での基本的なディレクトリ操作と権限設定
・Webサーバー（Python/Nginx）の基本的な仕組み
・GitHubを用いたコード・設定ファイルの管理
・SSH接続やサーバー設定手順の理解

## 苦労したこと
・ファイルの場所や内容が把握できず混乱
　→'ls -l', 'pwd', 'tree' コマンドを活用して、ディレクトリ構成とファイルの所在を確認
  → '/home/manato/server-study/html/index.html' がどこにあるか、Nginx からアクセスできるかを確認
・Nginxの root 設定と symbolic link の作成で 404 や 403 エラーが発生
　→ホームディレクトリに置いた HTML は表示されず、/var/www に移動して解決
・ファイルとディレクトリの所有者・権限の設定が不十分でアクセス拒否になった
　→'www-data' ユーザーに権限を与えて解決

## 今後の課題
・AWS 上でのサーバー構築
・Nginx を用いた実運用環境での公開
・HTTPS 設定やセキュリティの強化

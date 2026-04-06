# Linux Server Practice

## 概要
Ubuntu環境上でWebサーバーを構築し、HTMLを配信する練習を行いました。  
Pythonの簡易HTTPサーバーに加え、Nginxを用いたWebサーバーの公開も経験しました。

## 環境
- OS: Ubuntu Server  
- 仮想環境: VirtualBox  

## ディレクトリ構成
server-study/
├── README.md        ← 全体説明
├── linux/
│   ├── config/
│   ├── html/
│   └── logs/
└── aws/
    └── (AWS構築用の資料を格納予定)


## 実装内容

1. Python HTTP サーバーでの配信  
   `python3 -m http.server 8000` を使用してローカルでHTMLを確認  

2. NginxによるWebサーバー構築  
   - `/var/www/server-study/` に HTML を配置  
   - `/etc/nginx/sites-available/server-study` に設定ファイルを作成  
   - `sites-enabled` に symbolic link を作成して有効化  
   - 所有者と権限を Nginx ユーザー（www-data）に設定  
   - ブラウザで `http://localhost/` にアクセスし、`Hello Linux Server` の表示を確認  

3. GitHubでの管理  
   - コミット履歴によってサーバー構築の過程を可視化  

## 学んだこと
- Linux環境での基本的なディレクトリ操作と権限設定  
- Webサーバー（Python / Nginx）の基本的な仕組み  
- GitHubを用いたコード・設定ファイルの管理  
- SSH接続やサーバー設定手順の理解  
- エラー発生時に自分で原因を調査し解決する力の重要性を実感  

## 苦労したこと
- ファイルの場所や内容が把握できず混乱  
  → `ls -l`, `pwd`, `tree` コマンドを活用してディレクトリ構成とファイルの所在を確認  

- Nginxの root 設定や symbolic link の作成で 404 / 403 エラーが発生  
  → HTMLを `/var/www/server-study/` に移動し、設定を見直すことで解決  

- ファイルとディレクトリの所有者・権限の設定が不十分でアクセス拒否が発生  
  → `www-data` ユーザーに権限を付与して解決  

## 今後の課題
- AWS（EC2）上でのサーバー構築  
- Nginx を用いた実運用環境での公開  
- HTTPS 設定やセキュリティの強化

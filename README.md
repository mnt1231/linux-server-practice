# Linux Server Practice

## 概要
Ubuntu環境およびAWS EC2上でWebサーバーを構築し、HTMLを配信する練習を行いました。  
Pythonの簡易HTTPサーバーに加え、Nginxを用いたWebサーバーの公開も経験しました。  
また、アクセスログの取得・リアルタイム監視に加え、エラーログを分離する仕組みを構築しました。

## 目的
Webサーバーの構築だけでなく、実際の運用を想定し、  
アクセスログの取得・監視を通じてトラブル発生時に原因を特定できる力を身につけることを目的としました。

## 環境
- OS: Ubuntu Server  
- 仮想環境: VirtualBox
- クラウド: AWS EC2 (t3.micro)  

## ディレクトリ構成
server-study/
├── README.md
├── linux/
│   ├── config/
│   ├── html/
│   │   └── index.html
│   └── logs/
│       ├── access.log
│       └── error.log
└── aws/
    └── (AWS構築用の資料を格納予定)

## 実装内容

1. Python HTTP サーバーでの配信  
   `python3 -m http.server 8000` を使用してローカルでHTMLを確認
   
2. アクセスログの取得と監視  
   - 標準出力をログファイルへリダイレクト  
     ```bash
     python3 -m http.server 8000 > ../logs/access.log 2>&1
     ```
   - 別ターミナルでリアルタイム監視  
     ```bash
     tail -f ~/server-study/linux/logs/access.log
     ```
   - HTTPステータスコード（200, 304, 404, 403）の挙動を確認
     
3. エラーログの分離  
   Pythonの簡易HTTPサーバーではアクセスログとエラーログが分離されないため、  
   grepコマンドを用いてエラー（404, 403）のみを抽出する仕組みを構築した  
    ```bash
   tail -f access.log | grep --line-buffered -E "404|403" >> error.log
   ```
   存在しないURL（例: `/aaaaaaa.html`）にアクセスし、
   意図的に404エラーを発生させることで、error.logに記録されることを確認した。

4. NginxによるWebサーバー構築  
   - `/var/www/server-study/` に HTML を配置  
   - `/etc/nginx/sites-available/server-study` に設定ファイルを作成  
   - `sites-enabled` に symbolic link を作成して有効化  
   - 所有者と権限を Nginx ユーザー（www-data）に設定  
   - ブラウザで `http://localhost/` にアクセスし、`Hello Linux Server` の表示を確認

5. AWS EC2上でのWebサーバー構築  
   - EC2インスタンス（Ubuntu）を作成  
   - セキュリティグループでSSH / HTTP / HTTPSを許可  
   - EC2 Instance Connectを用いてSSH接続  
   - nginxをインストール  
     ```bash
     sudo apt update
     sudo apt install nginx -y
     ```
   - インターネット上からアクセス可能なWebサーバーを公開
   - `/var/www/html/index.nginx-debian.html` を編集しHTMLを変更  
   - ブラウザでパブリックIPにアクセスし、ページ表示を確認  

6. GitHubでの管理  
   - コミット履歴によってサーバー構築の過程を可視化  

## 学んだこと
- Linux環境での基本的なディレクトリ操作と権限設定  
- Webサーバー（Python / Nginx）の基本的な仕組み
- tailコマンドによるリアルタイムログ監視
- HTTPステータスコード (200, 304, 404, 403) の理解
- ログの分離とエラー抽出 (grepを用いてログ加工) 
- GitHubを用いたコード・設定ファイルの管理  
- SSH接続やサーバー設定手順の理解  
- エラー発生時に自分で原因を調査し解決する力の重要性を実感
- クラウド環境 (AWS) 上でもサーバー構築が可能であることを理解
- パブリックIPを用いた外部公開の仕組みを理解
- インスタンスの起動・停止の重要性

## 苦労したこと
- ファイルの場所や内容が把握できず混乱  
  → `ls -l`, `pwd`, `tree` コマンドを活用してディレクトリ構成とファイルの所在を確認  

- Nginxの root 設定や symbolic link の作成で 404 / 403 エラーが発生  
  → HTMLを `/var/www/server-study/` に移動し、設定を見直すことで解決  

- ファイルとディレクトリの所有者・権限の設定が不十分でアクセス拒否が発生  
  → `www-data` ユーザーに権限を付与して解決

- ログが別のターミナルにしか出力されず、ファイルに保存できなかった
  → リダイレクト (`>`, `2>&1`) の理解により解決

## 今後の課題
- AWS環境におけるWebサーバーの運用・改善
- Nginx を用いた実運用環境での公開  
- HTTPS 設定やセキュリティの強化

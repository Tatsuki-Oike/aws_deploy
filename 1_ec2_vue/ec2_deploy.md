# 1 EC2の準備

## 1.1 EC2の起動

* EC2 > インスタンス > インスタンス起動
  * 名前入力
  * キーペア作成
  * HTTPのトラフィック許可
  * インスタンス起動
* 全てのインスタンス表示 > 作成したインスタンス選択
* パブリック IPv4 アドレス確認

## 1.2 EC2の設定

* セキュリティ > セキュリティグループ > インバウンドルール
* インバウンドルールを編集 > ルールを追加
  * カスタムTCP
  * ポート範囲: 3000
  * ソース: 0.0.0.0/0
  * ルールを保存

## 1.3 EC2へ接続

* VSCodeを開く
* sshの拡張機能を利用
* configファイルに以下を記述
* ssh接続


```sh
Host remotessh
  HostName X.X.X.X (パブリックIP)
  User ec2-user
  Port 22
  IdentityFile X.pem (Keyのパス)
```

<br>

# 2 Vite Project

## 2.1 Node.js Install

* Node.js Install
```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
. ~/.nvm/nvm.sh
nvm install --lts
node -v
```

## 2.2 Vite Project

* Vite Project作成・サーバー起動
```sh
npm init vite-app vite_proj # プロジェクト(フォルダ)作成
cd vite_proj # 現在のディレクトリ移動
npm install # 必要なものをインストール
npm run dev # サーバー立てる
```

* http://X.X.X.X:3000 (パブリックIP:3000)

## 2.3 WEBサーバーでアクセス

* サーバーを止める
* WEBサーバーの起動
```sh
npm run build # buildファイルを作成する
sudo yum install httpd -y # Apache Install
sudo systemctl start httpd # Apache 起動
sudo cp -r dist/* /var/www/html # WEBアプリフォルダ移動
```

* http://X.X.X.X (パブリックIP)

<br>

# 3 後処理

* インスタンスの状態 > インスタンス停止
* インスタンスの状態 > インスタンス消去
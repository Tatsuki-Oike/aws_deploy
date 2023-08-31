# 1 Vite Project作成

* node.jsのインストール <br>
https://nodejs.org/en/download

* vue cliのインストール

```sh
npm install -g @vue/cli
```

* Vite Project作成

```sh
cd ./2_s3_vue
npm init vite-app vite_proj # プロジェクト(フォルダ)作成
cd vite_proj # 現在のディレクトリ移動
npm install # 必要なものをインストール
npm run build # buildファイルを作成する
```

<br>

# 2 S3 の作成

## 2.1 S3の設定

* バッケットを作成
* バケット設定
  * 「パブリックアクセスをすべて ブロック」のチェックを外す
  * バケット作成
* 「プロパティ」>「静的ウェブサイトホスティング」> 「編集」
  * 有効にする
  * インデックスドキュメント: index.html
  * 変更の保存
* 「アクセス許可」>「バケットポリシー」>「編集」
  * 以下のコードをコピペ
  * 変更を保存

```sh
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::[bucket-name]/*"
            ]
        }
    ]
}
```

## 2.2 WEBアプリのアップロード

* オブジェクト > アップロード
* distフォルダの中身をアップロード

<br>

# 3 WEBアプリにアクセス

* プロパティ > 静的ウェブサイトホスティングのURLにアクセス

<br>

# 4 後処理

* バケット選択 > 空にする
* バケット選択 > 消去
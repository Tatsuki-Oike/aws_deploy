# 1 EC2の準備

## 1.1 EC2の起動

* EC2 > インスタンス > インスタンス起動
  * キーペア作成
  * HTTP, HTTPSのトラフィック許可
  * インスタンス起動
* 全てのインスタンス表示 > 作成したインスタンス選択
* パブリック IPv4 アドレス確認

## 1.2 EC2へ接続

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

# 2 Flask App

* 仮想環境の作成

```sh
python3 -m venv venv # 仮想環境作成
source venv/bin/activate # 環境の中にはいる
python3 -m pip install --upgrade pip # pip upgrade
pip3 install flask # ライブラリインストール
pip3 install flask-restful # ライブラリインストール
```

* api.pyファイル作成

```python
from flask import Flask
from flask_restful import Resource, Api
import json

app = Flask(__name__)
api = Api(app)

class HelloWorld(Resource):
    def get(self):
        response = {'msg': 'Hello world'}
        return json.dumps(response)
    
api.add_resouばrce(HelloWorld, '/')

if __name__ == '__main__':
    app.run(debug=False, host='0.0.0.0', port=80)
```

* サーバーの起動

```sh
sudo systemctl stop httpd # サーバーを起動していた場合
sudo venv/bin/python api.py
```

* http://X.X.X.X (パブリックIP)

<br>

# 3 後処理

* インスタンスの状態 > インスタンス停止
* インスタンスの状態 > インスタンス消去
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

# 2 Vue.js

## 2.1 Node Install

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
npm init vite-app frontend # プロジェクト(フォルダ)作成
cd frontend # 現在のディレクトリ移動
npm install # 必要なものをインストール
npm install axios # 必要なものをインストール
```

## 2.3 App.vueファイル

* frontend/src/App.vue 変更後
```html
<template>
  <h1>Hello, World</h1>
  {{ response }}
  
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data() {
    return {
      response: {}
    }
  },
  mounted(){
    axios
      .get('/api/msg')
      .then(response => {
        this.response = JSON.parse(response.data)
      })
      .catch(error => {
        console.log(error)
      })
  },
}
</script>
```

## 2.4 build

* frontend/vite.config.js作成

```js
module.exports = {
    assetsDir: "static",
  }
```

* ビルド
```sh
npm run build # buildファイルを作成する
```

<br>

# 3 Flask

* 仮想環境の作成

```sh
cd .. # ディレクトリ移動
python3 -m venv venv # 仮想環境作成
source venv/bin/activate # 環境の中にはいる
python3 -m pip install --upgrade pip # pip upgrade
pip3 install flask # ライブラリインストール
pip3 install flask-restful # ライブラリインストール
pip3 install flask-cors # ライブラリインストール
```

* api.pyファイル作成

```python
from flask import Flask, render_template
from flask_restful import Resource, Api
import json
from flask_cors import CORS

app = Flask(__name__,
    static_folder="./frontend/dist/static",
    template_folder="./frontend/dist")
CORS(app)
api = Api(app)

class HelloWorld(Resource):
    def get(self):
        response = {'msg': 'Hello world'}
        return json.dumps(response)
    
api.add_resource(HelloWorld, '/api/msg')

@app.route('/', defaults={'path': ''})
@app.route('/<path:path>')
def index(path):
    return render_template('index.html')

if __name__ == '__main__':
    app.run(debug=False, host='0.0.0.0', port=80)
```

* サーバーの起動

```sh
sudo systemctl stop httpd # サーバーを起動していた場合
sudo venv/bin/python api.py
```

* http://X.X.X.X

<br>

# 4 後処理

* インスタンスの状態 > インスタンス停止
* インスタンスの状態 > インスタンス消去
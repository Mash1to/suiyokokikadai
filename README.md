# 水曜1，2限後期課題

## Dockerのインストール方法

### パッケージの最新化
```
sudo dnf update -y
```

### Gitのインストール
```
sudo dnf install -y git　
```

### Dockerのインストール
```
sudo dnf install -y docker
```

### Dockerの自動起動の有効化
```
sudo systemctl enable docker
```

### Dockerサービスを起動
```
sudo systemctl start docker
```

### Docker Compose v2プラグインの配置先を作る
```
sudo mkdir -p /usr/local/lib/docker/cli-plugins/
```

### Docker Compose v2をダウンロードして配置
```
sudo curl -SL https://github.com/docker/compose/releases/download/v2.36.0/docker-compose-linux-x86_64 -o /usr/local/lib/docker/cli-plugins/docker-compose
```

### ダウンロードしたComposeを実行可能にする
```
sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
```

### ec2-userをdockerグループに追加
```
sudo usermod -aG docker ec2-user
```

再ログイン後に確認:

docker --version

docker compose version

gitからソースコードを取得

git clone https://github.com/Mash1to/suiyokokikadai.git

cd suiyokokikadai

Docker コンテナのビルド・起動

docker compose up -d --build

正常に起動すると、以下のようなコンテナが起動する。

docker compose ps

mysql入力 　　　sudo docker compose exec mysql mysql example_db

# 水曜1，2限　後期課題 手順書
## 0.概要
本手順書は、EC2（Amazon Linux系）上でDockerを用いてアプリケーションを起動するまでの手順をまとめたものです。

## 1.Dockerのインストール方法

### 1-1.パッケージの最新化
```
sudo dnf update -y
```

### 1-2.Gitのインストール
```
sudo dnf install -y git
```

### 1-3.Dockerのインストール
```
sudo dnf install -y docker
```

### 1-4.Dockerの自動起動の有効化
```
sudo systemctl enable docker
```

### 1-5.Dockerサービスを起動
```
sudo systemctl start docker
```

### 1-6.Docker Compose v2プラグインの配置先を作る
```
sudo mkdir -p /usr/local/lib/docker/cli-plugins/
```

### 1-7.Docker Compose v2をダウンロードして配置
```
sudo curl -SL https://github.com/docker/compose/releases/download/v2.36.0/docker-compose-linux-x86_64 -o /usr/local/lib/docker/cli-plugins/docker-compose
```
今回はv2.36.0を入れていますが、最新版はv5.0.2です。

### 1-8.ダウンロードしたComposeを実行可能にする
```
sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
```

### 1-9.ec2-userをdockerグループに追加
```
sudo usermod -aG docker ec2-user
```

### 1-10.再ログイン後に確認:
```
docker --version

docker compose version
```

## 2.ソースコードの配置方法
今回はgit cloneでGithubからソースコードを取得します。
```
git clone https://github.com/Mash1to/suiyokokikadai.git

cd suiyokokikadai
```

## 3.Docker コンテナのビルド・起動
```
docker compose up -d --build
```
正常に起動すると、以下のようなコンテナが起動する。
```
docker compose ps
```
mysql入力 　　　sudo docker compose exec mysql mysql example_db

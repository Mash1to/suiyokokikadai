# 水曜1，2限　後期課題 手順書
## 0.概要
本手順書は、EC2でDockerを用いてSNS風アプリケーションを起動するまでの手順をまとめたものです。

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
```
```
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

## 4.DBのテーブルの作成方法

### 4-1.データベースに入る
```
sudo docker compose exec mysql mysql example_db
```

### 4-2.SQL作成
```sql
CREATE TABLE `users` (
  `id` INT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY,
  `name` TEXT NOT NULL,
  `email` TEXT NOT NULL,
  `password` TEXT NOT NULL,
  `created_at` DATETIME DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE `users` ADD COLUMN icon_filename TEXT DEFAULT NULL;

ALTER TABLE `users` ADD COLUMN introduction TEXT DEFAULT NULL;

ALTER TABLE `users` ADD COLUMN cover_filename TEXT DEFAULT NULL;

ALTER TABLE `users` ADD COLUMN birthday DATE DEFAULT NULL;
```

```sql
CREATE TABLE `bbs_entries` (
  `id` INT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY,
  `user_id` INT UNSIGNED NOT NULL,
  `body` TEXT NOT NULL,
  `image_filename` TEXT DEFAULT NULL,
  `created_at` DATETIME DEFAULT CURRENT_TIMESTAMP
);
```
```sql
CREATE TABLE `access_logs` (
  `id` INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
  `user_agent` TEXT NOT NULL,
  `remote_ip` TEXT NOT NULL,
  `created_at` DATETIME DEFAULT CURRENT_TIMESTAMP
);
```
```sql
CREATE TABLE `user_relationships` (
  `id` INT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY,
  `followee_user_id` INT UNSIGNED NOT NULL,
  `follower_user_id` INT UNSIGNED NOT NULL,
  `created_at` DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

### 5.起動後
アクセス　(パブリック IPv4 アドレス)/login.php

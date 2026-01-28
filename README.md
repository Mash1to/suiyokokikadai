sudo dnf update -y

sudo dnf install -y git　

sudo dnf install -y docker

sudo systemctl enable docker

sudo systemctl start docker

sudo mkdir -p /usr/local/lib/docker/cli-plugins/

sudo curl -SL https://github.com/docker/compose/releases/download/v2.36.0/docker-compose-linux-x86_64 -o /usr/local/lib/docker/cli-plugins/docker-compose

sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-compose

sudo usermod -aG docker ec2-user

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

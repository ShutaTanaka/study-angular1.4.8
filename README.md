# AngularJS環境構築

- 参考サイト
  - https://qiita.com/PRONakahira/items/f507d0f912974d1b8c58
  - ここではnodeとangularバージョンの指定方法は書かれていないので、以下に指定方法を加えた方法を追記する。
  - dockerの起動以降は、参考サイトを参考に進めれば良い。

1. 任意のディレクトリに、当リポジトリをクローンする。
2. Dockerfileを書き換えてnodeとAngularJSのバージョンを指定する。<br>最新バージョンで良い場合は、書き換える必要はない。
```
ディレクトリ構成
.
├── docker
│   └── node
│       └── Dockerfile【書き換え対象】
└── docker-compose.yml
```
`Dockerfileの書き換え方`
```
FROM node【「:」の後にバージョンを追記】
WORKDIR /projects
【「RUN npm config set unsafe-perm true」を追記】
RUN npm install -g @angular/cli【「＠」の後にバージョンを追記】
【「RUN apk add bash」を追記】
EXPOSE 4200
```

`【例】 [node]8.12.0-alpine、[angular cli] 6.2.6`
```
FROM node:8.12.0-alpine
WORKDIR /projects
RUN npm config set unsafe-perm true
RUN npm install -g @angular/cli@6.2.6
RUN apk add bash
EXPOSE 4200
```

3. docker-compose.yml のあるディレクトリで以下のコマンドを実行し起動。

```bash
docker-compose up -d
```
- Dockerコンテナがビルドされているか以下コマンドで確認。
```bash
docker ps
```

## 起動に失敗した場合
- 起動の際に以下のようなエラーが出た場合、docker/config.jsonを書き換えることで解消した。
```
❯ sudo docker-compose up -d
[+] Building 0.5s (2/3)
[+] Building 0.6s (3/3) FINISHED
 => [node internal] load build definition from Dockerfile                                   0.0s
 => => transferring dockerfile: 392B                                                        0.0s
 => [node internal] load .dockerignore                                                      0.0s
 => => transferring context: 2B                                                             0.0s
 => ERROR [node internal] load metadata for docker.io/library/node:latest                   0.6s
------
 > [node internal] load metadata for docker.io/library/node:latest:
------
failed to solve: node: error getting credentials - err: exit status 1, out: ``
```
- config.json書き換え方
  1. PCの~/.docker/config.jsonを開く。
  2. 「credsStore」が書かれている行ごと消す。
  3. 再度起動を試みる。


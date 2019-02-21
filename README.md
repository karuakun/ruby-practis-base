# このリポジトリは？

Rails の開発を Docker 出始めるための手順をまとめたリポジトリです。
開発者はこのリポジトリを clone して下記の手順を実施すると、ruby の設定を行うことなく開発を開始することができます。

## 前提

下記のソフトウェアがインストールされていることを前提にしています。

- git がインストールされている
- Docker(for Desktop)がインストールされ、git hubにログインしている

## このプロジェクトの構成

詳細は /src/Dockerfile および /src/Gemfile を参照してください。

### このプロジェクトで利用するソフトウェアのバージョン

- ruby:2.4.3-alpine 
- rails 5.2.2
- mysql 5.7

### バインドするポート

- rails 5000
- mysql 3306

## セットアップ

Railsのプロジェクトを作成、起動しウェルカムページが表示されることを確認します。

### セットアップに必要なファイル一式をダウンロードする

``` shell
git clone https://github.com/karuakun/ruby-practis-base.git
cd ruby-practis-base
```

### Railsのプロジェクトを作る

``` shell
docker-compose run --rm app bundle exec rails new . --force --database=mysql --skip-bundle
```

### `データベース設定をコンテナにあわせて書き換える

テキストエディターで`src/config/database.yml`を開き、default定義を下記のように書き換えます。

``` yml
default: &default
  adapter: mysql2
  encoding: utf8
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: <%= ENV['DATABASE_USER'] || 'root' %>
  password: <%= ENV['DATABASE_PASSWORD'] || '' %>
  host: <%= ENV['DATABASE_HOST'] || 'localhost' %>
```

### コンテナをビルドして起動する

``` shell
docker-compose build --no-cache
docker-compose up
```

### データベースを作る

``` shell
docker-compose run --rm app rails db:create
```

### Webブラウザーで接続して確認する

http://localhost:3000

### コンテナを停止する

``` shell
docker-compose stop
```

### コンテナを破棄する

``` shell
docker-compose down -v
```

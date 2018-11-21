# Rails ローカル開発環境構築

docker-compose を利用し、ローカル開発環境の構築しています。

基本的に `docker-compose up` すれば起動します。

ドメインは `docker/development/nginx/ssl/setup.sh` の `SSL_DOMAIN` で指定しています。
ドメイン変更したい場合は、 *SSL 生成* を参照して実行してください。


## 想定環境

* MacOS
* Docker for Mac 2.0.0.0-mac78 (28905)

## 初期ファイル

* docker/*
* docker-compose.yml

## Gemfile 導入

```
docker-compose run --rm --no-deps web bundle init
```

## Rails インストール

```
docker-compose run --rm --no-deps web bundle install -j6 --path vendor/bundle
```

## Rails プロジェクト作成

```
docker-compose run --rm --no-deps web bundle exec rails new . -B -d mysql --skip-turbolinks --skip-test
```

#### option の意味

* -B ... Rails プロジェクト作成時に生成される Gemfile の gem をインストールしない。
* -d mysql ... DB に mysql を採用
* --skip-turbolinks ... turbolinks 導入しない
* --skip-test ... test を導入しない。 minitest, rspec とその後導入するか決めるつもり。

## gem インストール

```
docker-compose run --rm --no-deps web bundle install -j6
```

## rubocop 実行

以下実行し、offence を潰す。

```
docker-compose run --rm --no-deps web bundle exec rubocop
```

## SSL 生成

* `*.hoge.test` というドメインでオレオレ ssl 作成

```
cd docker/development/nginx/ssl && ./setup.sh
```

## /etc/hosts 設定

```
# /etc/hosts

127.0.0.1 dev.hoge.test
```

## config/database.yml 設定

RAILS_ENV=development をまず設定

## DB 作成

```
docker-compose run --rm web bundle exec rake db:create RAILS_ENV=development
```

```
docker-compose run --rm web bundle exec rake db:migrate
```

## 起動してみる。

```
docker-compose up
```

ブラウザから https://dev.hoge.test

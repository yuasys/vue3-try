# README.md

このリポジトリは主に[この動画](https://youtu.be/1smtU3CbP34)で、「Vue.js／Vue3入門」を学習した体験を記録したものです。  

文法エラーや間違った記述も含まれていますので、ご覧の方はご注意ください。  

## 関連サイト

https://reisuta.com/vue3-init/

## 環境構築

### 1. お手本をCloneして、プロジェクト名を決め、VSCodeで開く

プロジェクトフォルダを置く基点となるフォルダ: ~/source/  
プロジェクト名（フォルダ名）: vue3-try  

```bash
 # プロジェクトフォルダを置く基点となるフォルダに移動する
$ cd ~/source

 # お手本リオ時取りをクローンする
$ git clone https://github.com/reisuta/Sigh.git

 # クローンしたプロジェクト名(フォルダ名）をsighから任意の名前に変更する
 # ここではプロジェクト名はvue3-tryとする
$ mv sigh vue3-try

 # プロジェクトに移動し、VSCodeを起動し、以降はVSCodeで作業する
$ cd vue3-try
$ code .
```

### 2.クローン（複製）したプロジェクトを修正する

#### （１）frontディレクトリを削除する

```bash
# プロジェクトルートに移動(していることを確認)
$ cd ~/source/vue3-try

 # frontディレクトリを全削除(注1)
$ rm -rf front 
```

注1: これは、bashの実行例。ターミナルがPowerShellの場合はエラーが吐き出されるので、単に`rm front`とする

#### （２）docker-compose.ymlを修正する

お手本ではNuxt.js用に作られているので、これをVue3用に修正する。  
※Nuxt3はまだ不安定なので、安定するのを待つ  

```yml
# ----------------------------
# 【修正後のdocker-compose.yml】
#-----------------------------
version: '3.9'
services:
  db:
    image: postgres
    volumes:
      - ./api/tmp/db:/var/lib/postgresql/data
    environment:
      POSTGRES_PASSWORD: password
  front:
    build: .
    working_dir: /app
    volumes:
      - .:/app
    ports:
      - "5173:5173"
  api:
    build: ./api
    command: bash -c "rm -f api/tmp/pids/server.pid && bundle exec rails s -p 4000 -b '0.0.0.0'"
    stdin_open: true
    tty: true
    volumes:
      - ./api:/myapp
    ports:
      - "4000:4000"
    depends_on:
      - db
```

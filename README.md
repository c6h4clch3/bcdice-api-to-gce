# BCDice-api for Google Cloud Endpoint
[BCDice-api](https://github.com/NKMR6194/bcdice-api)をGoogle Cloud Endpointで扱うための設定ファイル群

## Dependent
- Ruby
- Bundler

インストール方法は各自で調べてください。

Google Cloud Platformへのデプロイに必要
- Google アカウント(GCP利用登録完了済み)
- gcloud コマンドラインツール

セットアップは[gcloud の概要](https://cloud.google.com/sdk/gcloud/)を参照。

ひとまずコンソールで`gcloud help`と打ってみてエラーが出なければインストールできてるので、後は参照先参考にセットアップ完了させてください。

## Setup
Projectのクローン
```sh
$ git clone https://github.com/c6h4clch3/bcdice-api-to-gce.git
```

Projectのセットアップ
```sh
$ cd bcdice-api-to-gce
$ cp app_example.yaml app.yaml
$ cp openapi.yaml openapi.yaml
$ git submodule update --init --recursive
$ bundle install
```

GCP プロジェクトのセットアップ
1. GCP コンソールを開く
1. 任意の名前でプロジェクトを作成する
1. プロジェクトIDを控える

Google Cloud Endpoint のセットアップ
1. openapi.yamlを以下のように編集

```yaml
...
host: "bcdice-api.endpoints.(控えたプロジェクトID).cloud.goog"
...
```

2. Google Cloud Endpointにデプロイ

```sh
$ gcloud service-management deploy openapi.yaml
```

3. サービスIDを控える

```sh
$ gcloud service-management configs list --service=bcdice-api.endpoints.(控えたプロジェクトID).cloud.goog
```

と入力して出力結果を控える

4. app.yamlを以下のように編集

```yaml
...
endpoints_api_service:
  name: bcdice-api.endpoints.(控えたプロジェクトID).cloud.goog
  config_id: (控えたサービスID)
```

5. Google App Engine にデプロイ
```sh
$ gcloud app deploy
```

6. 動作確認

```
http://(控えたプロジェクトID).appspot.com/vi/version?callback=hogehoge
```

にアクセスし、レスポンスが返却されることを確認

## Thanks To
- [BCDice](https://github.com/torgtaitai/BCDice)
- [BCDice-api](https://github.com/NKMR6194/bcdice-api)

## Author
ミナト @c6h4clch3

- Twitter: @c6h4clch3
- Qiita: @c6h4clch3_gh
- Qiitadon: @c6h4clch3_gh

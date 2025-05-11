# prometheus-grafana起動

Spring Bootで手早くメトリクスを確認したい人のための手順

## 前提

1. Dockerはホストマシン上にインストール済み

1. Spring Bootは構築済み（以下2つを依存関係に追加し、<http://localhost:8090/actuator/prometheus>等で動作確認済みであること。見て分かる通り、当手順ではホストマシン上でSpringを動かしている）
>  -  spring-boot-starter-actuator
>  -  micrometer-registry-prometheus

この手順は以下のqiitaのページを参考にしている。

https://qiita.com/YoheiNakanishi/items/fed742232787f8dbda89

## 手順
### ホストマシンにプロジェクトを展開

ホストマシン上の任意の場所にこのプロジェクトを展開する

### ymlファイルの編集

prometheus.ymlのglobal.scrape_configsのstatic_configs.targetsのアドレスを編集する。
ifconfig等でホストマシンのIPアドレスを調べ、記載する。

### docker起動

プロジェクトルートに移動し、以下コマンドでdocker起動（-fオプションは指定しなくても良いはず）
```
docker compose -f ./compose.yml up -d
```

### 【動作確認】grafana管理コンソールにアクセス
ホストマシン上から、http://localhost:3001にアクセスする。
ログイン画面が表示されればOK
grafana.ymlに記載しているユーザ情報でログインする。

起動できなかった場合は、dockerのデーモン起動をやめてちゃんとログを見る。
```
docker compose -f ./compose.yml up 
```

#### ダッシュボードの設定

grafanaの画面（http://localhost:3001）から、左ペインのDashboardsを選択、「New」ボタンにImportがあるので、そいつを選択してJSONを指定する。
14430_rev1.json

#### N/Aになる場合

ダッシュボードの項目すべてがN/Aになっている場合、prometheusがState: UPになっているか確認する。
http://localhost:9090/targets

# Elasticsearch＋KibanaをMac上で試してみる（サンプルデータ編）

# 概要
気軽にElasticsearchを試してみたく、Docker for Macを使用してサクッと起動してみた際のDocker設定。  
この時点ではどういう点で生きてくるかわからないが、マルチノードのElasticsearchとKibanaを使用。  
データは予め準備したりせず、まずはサンプルデータを使用。

# 公式ページ
[Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html)  
[Kibana](https://www.elastic.co/guide/en/kibana/current/docker.html)

# 手順
## Docker for Macをインストール
手順に関してはここで書いておきたい内容とは逸れるため割愛。  
[公式ページ](https://docs.docker.com/docker-for-mac/install/)参照

※ Elasticsearch公式ページの記載より、メモリの割当サイズを最低4GiB以上にする点に注意

## Macローカルの任意のディレクトリにdocker-compose.ymlを作成

各コンテナのnetworksの指定に注意。この指定がされていないコンテナはコンテナ間の接続ができない。  
（実際にKibanaとElasticSearchがうまく繋げなくて躓いた…）

また、ELASTICSEARCH_HOSTSのホスト名は9200ポートで接続ができるコンテナのコンテナ名を指定する。  
ここでlocalhostを指定すると、Kibanaが起動しているコンテナ自身を指すことになり接続ができない。

KibanaはOSSイメージではモニタリングプラグインは定義されていないようなので、ここでは何も考えずモニタリングのパラメータは無効に変更。

## docker起動

```sh
docker-compose up
```

## 起動確認

Elasticsearch: [http://localhost:9200/](http://localhost:9200/)  
Kibana: [http://localhost:5601/](http://localhost:5601/)

## 日本語対応
日本語を使用できるようにするため、analysis-kuromojiやanalysis-icuをelasticsearchのコンテナにインストールする

## プラグインインストール結果確認

```sh
curl -H "Content-Type: application/json"  -XGET 'http://localhost:9200/_nodes/plugins' | jq
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 41412  100 41412    0     0  3998k      0 --:--:-- --:--:-- --:--:-- 4044k
{
  "_nodes": {
    "total": 3,
    "successful": 3,
    "failed": 0
  },
  "cluster_name": "es-docker-cluster",
  "nodes": {
    "FJEOAd3ISyKTLJCWvdZeJw": {
      "name": "es03",
      "transport_address": "172.21.0.3:9300",
      "host": "172.21.0.3",
      "ip": "172.21.0.3",
      "version": "7.7.0",
      "build_flavor": "default",
      "build_type": "docker",
      "build_hash": "81a1e9eda8e6183f5237786246f6dced26a10eaf",
      "roles": [
        "ingest",
        "master",
        "transform",
        "data",
        "remote_cluster_client",
        "ml"
      ],
      "attributes": {
        "ml.machine_memory": "4129218560",
        "ml.max_open_jobs": "20",
        "xpack.installed": "true",
        "transform.node": "true"
      },
      "plugins": [
        {
          "name": "analysis-icu",
          "version": "7.7.0",
          "elasticsearch_version": "7.7.0",
          "java_version": "1.8",
          "description": "The ICU Analysis plugin integrates the Lucene ICU module into Elasticsearch, adding ICU-related analysis components.",
          "classname": "org.elasticsearch.plugin.analysis.icu.AnalysisICUPlugin",
          "extended_plugins": [],
          "has_native_controller": false
        },
        {
          "name": "analysis-kuromoji",
          "version": "7.7.0",
          "elasticsearch_version": "7.7.0",
          "java_version": "1.8",
          "description": "The Japanese (kuromoji) Analysis plugin integrates Lucene kuromoji analysis module into elasticsearch.",
          "classname": "org.elasticsearch.plugin.analysis.kuromoji.AnalysisKuromojiPlugin",
          "extended_plugins": [],
          "has_native_controller": false
        }
      ],
（以下省略）
```


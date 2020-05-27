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
日本語を使うならanalysis-kuromojiやanalysis-icuが必要になりそう
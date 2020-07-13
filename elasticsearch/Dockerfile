FROM docker.elastic.co/elasticsearch/elasticsearch:7.8.0

# 日本語解析用plugin
RUN elasticsearch-plugin install analysis-kuromoji
RUN elasticsearch-plugin install analysis-icu
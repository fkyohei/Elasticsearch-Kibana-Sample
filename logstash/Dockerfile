FROM docker.elastic.co/logstash/logstash:7.8.0

RUN rm -f /usr/share/logstash/pipeline/logstash.conf
ADD pipeline/ /usr/share/logstash/pipeline/

RUN logstash-plugin install logstash-filter-translate

COPY mysql-connector-java-8.0.20.jar /usr/share/logstash/mysql-connector-java-8.0.20.jar
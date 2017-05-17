FROM openjdk:8-jre

ENV ES_PKG_VERSION 5.4.0
ENV ES_HOME /usr/share/elasticsearch

RUN set -x \
    \
# Download Elasticsearch .tar.gz package and verify
    && cd /tmp \
    && wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-${ES_PKG_VERSION}.tar.gz \
    && EXPECTED_SHA=$(wget -O - https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-${ES_PKG_VERSION}.tar.gz.sha1) \
    && test ${EXPECTED_SHA} = $(sha1sum elasticsearch-${ES_PKG_VERSION}.tar.gz | awk '{print $1}') \
    \
# Install & Configure
    && tar -zxf /tmp/elasticsearch-${ES_PKG_VERSION}.tar.gz \
    && mv /tmp/elasticsearch-${ES_PKG_VERSION} ${ES_HOME} \
    && sed -i "s/#network.host: 192.168.0.1/network.host: 0.0.0.0/g" ${ES_HOME}/config/elasticsearch.yml \
    && sed -i "s/Xms2g/Xms200m/g; s/Xmx2g/Xmx200m/g" ${ES_HOME}/config/jvm.options \
    \
# Create elasticsearch usergroup
    && groupadd elasticsearch \
    && useradd -g elasticsearch elasticsearch \
    && chown -R elasticsearch:elasticsearch ${ES_HOME} \
    \
# Clean up
    && rm -rf /tmp/*.tar.gz

USER elasticsearch
WORKDIR ${ES_HOME}
EXPOSE 9200 9300

CMD ["bin/elasticsearch"]

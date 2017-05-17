FROM openjdk:8-jre

ENV KIBANA_VERSION 5.4.0
ENV KIBANA_HOME /usr/share/kibana

RUN set -x \
    \
# Download Kibana .tar.gz package and verify
    && cd /tmp \
    && wget https://artifacts.elastic.co/downloads/kibana/kibana-${KIBANA_VERSION}-linux-x86_64.tar.gz \
    && EXPECTED_SHA=$(wget -O - https://artifacts.elastic.co/downloads/kibana/kibana-${KIBANA_VERSION}-linux-x86_64.tar.gz.sha1) \
    && test ${EXPECTED_SHA} = $(sha1sum kibana-${KIBANA_VERSION}-linux-x86_64.tar.gz | awk '{print $1}') \
    \
# Install & Configure
    && tar -zxf /tmp/kibana-${KIBANA_VERSION}-linux-x86_64.tar.gz \
    && mv /tmp/kibana-${KIBANA_VERSION}-linux-x86_64 ${KIBANA_HOME} \
    && sed -i 's|#server.host: "localhost"|server.host: "0.0.0.0"|g; s|#elasticsearch.url: "http://localhost:9200"|elasticsearch.url: "http://elasticsearch:9200"|g' ${KIBANA_HOME}/config/kibana.yml \
    \
# Create kibana usergroup
    && groupadd kibana \
    && useradd -g kibana kibana \
    && chown -R kibana:kibana ${KIBANA_HOME} \
    \
# Clean up
    && rm -rf /tmp/*.tar.gz

USER kibana
WORKDIR ${KIBANA_HOME}
EXPOSE 5601

CMD ["bin/kibana"]

FROM openjdk:8-jre

ENV SCALA_VERSION 2.11
ENV KAFKA_VERSION 0.10.2.0
ENV KAFKA_HOME /opt/kafka_${SCALA_VERSION}-${KAFKA_VERSION}

RUN set -x \
    \
# Download Kafka files. TODO: VERIFY KEYS.
    && cd /tmp \
    && wget http://mirrors.advancedhosters.com/apache/kafka/${KAFKA_VERSION}/kafka_${SCALA_VERSION}-${KAFKA_VERSION}.tgz \
    \
# Install & Configure
    && tar -xzf /tmp/kafka_${SCALA_VERSION}-${KAFKA_VERSION}.tgz -C /opt \
    && sed -i "s/zookeeper.connect=localhost:2181/zookeeper.connect=zookeeper:2181/g" ${KAFKA_HOME}/config/server.properties \
    && sed -i "s|#advertised.listeners=PLAINTEXT://your.host.name:9092|advertised.listeners=PLAINTEXT://localhost:9092|g" ${KAFKA_HOME}/config/server.properties \
    \
# Use wait-for-it script to control startup order
    && wget -O /usr/local/bin/wait-for-it https://raw.githubusercontent.com/vishnubob/wait-for-it/master/wait-for-it.sh \
    && chmod +x /usr/local/bin/wait-for-it \
    \
# Clean up
    && rm -rf /tmp

WORKDIR ${KAFKA_HOME}
VOLUME ["${KAFKA_HOME}/config"]
EXPOSE 9092

CMD  wait-for-it -h zookeeper -p 2181 -s -t 10 -- bin/kafka-server-start.sh config/server.properties

FROM openjdk:8-jre

ENV ZOOKEEPER_VERSION 3.4.9
ENV ZOOKEEPER_HOME /opt/zookeeper-${ZOOKEEPER_VERSION}

RUN set -x \
    \
# Download ZooKeeper files and verify
    && cd /tmp \
    && wget http://mirrors.advancedhosters.com/apache/zookeeper/zookeeper-${ZOOKEEPER_VERSION}/zookeeper-${ZOOKEEPER_VERSION}.tar.gz \
    && wget http://www-eu.apache.org/dist/zookeeper/KEYS \
    && wget http://www-eu.apache.org/dist/zookeeper/zookeeper-${ZOOKEEPER_VERSION}/zookeeper-${ZOOKEEPER_VERSION}.tar.gz.asc \
    && gpg --import KEYS \
    && gpg --verify zookeeper-${ZOOKEEPER_VERSION}.tar.gz.asc zookeeper-${ZOOKEEPER_VERSION}.tar.gz \
    \
# Install & Configure
    && tar -zxf /tmp/zookeeper-${ZOOKEEPER_VERSION}.tar.gz -C /opt \
    && mv ${ZOOKEEPER_HOME}/conf/zoo_sample.cfg ${ZOOKEEPER_HOME}/conf/zoo.cfg \
    && sed -i "s|/tmp/zookeeper|${ZOOKEEPER_HOME}/data|g" ${ZOOKEEPER_HOME}/conf/zoo.cfg \
    && mkdir ${ZOOKEEPER_HOME}/data \
    \
# Clean up
    && rm -rf /tmp

WORKDIR ${ZOOKEEPER_HOME}
VOLUME ["${ZOOKEEPER_HOME}/conf", "${ZOOKEEPER_HOME}/data"]
EXPOSE 2181 2888 3888

CMD bin/zkServer.sh start-foreground

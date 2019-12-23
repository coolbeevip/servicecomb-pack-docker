FROM maven:3.6.0-jdk-8 as builder

RUN git clone https://github.com/apache/servicecomb-pack.git \
  && cd servicecomb-pack \
  && mvn clean package -f alpha -DskipTests=true

FROM postgres:9.6-alpine

LABEL description="Apache ServiceComb Pack"
LABEL maintainer="coolbeevip@gmail.com"
ENV ALPHA_VERSION=0.6.0-SNAPSHOT
ENV POSTGRES_USER=saga
ENV POSTGRES_PASSWORD=password
ENV POSTGRES_DB=saga
ENV ELASTICSEARCH_VERSION=6.6.2
ENV ALPHA_MODEL=AKKA

COPY startup.sh /
RUN mkdir /servicecomb-pack /servicecomb-pack/plugins /servicecomb-pack/logs
COPY --from=builder /servicecomb-pack/alpha/alpha-server/target/saga/alpha-server-${ALPHA_VERSION}-exec.jar /servicecomb-pack/alpha-server-exec.jar
COPY --from=builder /servicecomb-pack/alpha/alpha-benchmark/target/saga/alpha-benchmark-${ALPHA_VERSION}-exec.jar /servicecomb-pack/alpha-benchmark-exec.jar

# Install Elasticsearch
# https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-${ELASTICSEARCH_VERSION}.tar.gz

RUN wget -O /elasticsearch-${ELASTICSEARCH_VERSION}.tar.gz https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-${ELASTICSEARCH_VERSION}.tar.gz \
  && tar -xzf elasticsearch-${ELASTICSEARCH_VERSION}.tar.gz \
  && rm elasticsearch-${ELASTICSEARCH_VERSION}.tar.gz \
  && mv elasticsearch-${ELASTICSEARCH_VERSION} elasticsearch \
  && mkdir -p /elasticsearch/data \
  && chown -R postgres:postgres /elasticsearch \
  && rm -rf /elasticsearch/modules/x-pack-ml


# Install Apache ServiceComb Pack Alpha

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories \
  && apk add openjdk8-jre \
  && chmod +x /startup.sh \
  && chown -R postgres:postgres /servicecomb-pack

WORKDIR /servicecomb-pack

EXPOSE 8080
EXPOSE 8090
EXPOSE 5432
EXPOSE 9200

VOLUME /servicecomb-pack/logs
VOLUME /servicecomb-pack/plugins
VOLUME /var/lib/postgresql/data

CMD ["/startup.sh"]
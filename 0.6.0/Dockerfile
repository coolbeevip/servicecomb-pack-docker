FROM maven:3.6.0-jdk-8 as builder

RUN git clone https://github.com/apache/servicecomb-pack.git \
  && cd servicecomb-pack \
  && mvn clean package -f alpha -DskipTests=true

FROM openjdk:8-jre-alpine3.9

LABEL description="Apache ServiceComb Pack"
LABEL maintainer="coolbeevip@gmail.com"
ENV ALPHA_VERSION=0.6.0-SNAPSHOT

RUN mkdir /servicecomb-pack /servicecomb-pack/plugins /servicecomb-pack/logs
COPY --from=builder /servicecomb-pack/alpha/alpha-server/target/saga/alpha-server-${ALPHA_VERSION}-exec.jar /servicecomb-pack/alpha-server-exec.jar
COPY --from=builder /servicecomb-pack/alpha/alpha-benchmark/target/saga/alpha-benchmark-${ALPHA_VERSION}-exec.jar /servicecomb-pack/alpha-benchmark-exec.jar

WORKDIR /servicecomb-pack

EXPOSE 8080
EXPOSE 8090

VOLUME /servicecomb-pack/logs
VOLUME /servicecomb-pack/plugins

CMD ["java","-Dloader.path=/servicecomb-pack/plugins","-jar","alpha-server-exec.jar","--spring.profiles.active=prd"]
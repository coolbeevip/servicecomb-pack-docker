FROM openjdk:8-jre-alpine3.9

LABEL description="Apache ServiceComb Pack"
LABEL maintainer="coolbeevip@gmail.com"
ENV ALPHA_VERSION=0.6.0

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories \
  && apk add --no-cache gnupg grep \
  && wget -T 600 -O /apache-servicecomb-pack-distribution-${ALPHA_VERSION}-bin.zip http://mirrors.tuna.tsinghua.edu.cn/apache/servicecomb/servicecomb-pack/${ALPHA_VERSION}/apache-servicecomb-pack-distribution-${ALPHA_VERSION}-bin.zip \
  && wget -T 600 -O /apache-servicecomb-pack-distribution-${ALPHA_VERSION}-bin.zip.asc https://www.apache.org/dist/servicecomb/servicecomb-pack/${ALPHA_VERSION}/apache-servicecomb-pack-distribution-${ALPHA_VERSION}-bin.zip.asc \
  && wget -T 600 -O /KEYS https://www.apache.org/dist/servicecomb/KEYS \
  && gpg --import /KEYS \
  && gpg --verify /apache-servicecomb-pack-distribution-${ALPHA_VERSION}-bin.zip.asc /apache-servicecomb-pack-distribution-${ALPHA_VERSION}-bin.zip \
  && unzip apache-servicecomb-pack-distribution-${ALPHA_VERSION}-bin.zip \
  && rm -rf /apache-servicecomb-pack-distribution-${ALPHA_VERSION}-bin.zip.asc \
  && rm -rf /apache-servicecomb-pack-distribution-${ALPHA_VERSION}-bin.zip \
  && rm -rf /KEYS \
  && mv apache-servicecomb-pack-distribution-${ALPHA_VERSION}-bin servicecomb-pack \
  && cd servicecomb-pack \
  && mkdir plugins \
  && mv alpha-server-${ALPHA_VERSION}-exec.jar alpha-server-exec.jar \
  && rm -rf `ls | egrep -v '(alpha-server-exec.jar|LICENSE|NOTICE)'` \
  && apk del gnupg grep

WORKDIR /servicecomb-pack

EXPOSE 8080
EXPOSE 8090

VOLUME /servicecomb-pack/logs
VOLUME /servicecomb-pack/plugins

CMD ["java","-Dloader.path=/servicecomb-pack/plugins","-jar","alpha-server-exec.jar","--spring.profiles.active=prd"]

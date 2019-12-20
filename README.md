 mvn clean install -DskipTests=true -Prelease -Dgpg.skip -Dmaven.javadoc.skip=true


# 编译

docker build -f Dockerfile -t coolbeevip/servicecomb-pack .
docker build -f Dockerfile.allinone -t coolbeevip/servicecomb-pack-allinone .

## 说明

* All-in-One: 包含 Pack 所需要的所有服务，只需要启动一个镜像即可（单点试用）
* 标准: 仅包含 Pack 服务，使用时需要单独部署其他依赖服务，例如 postgres elasticsearch 等（可用于集群部署）

## 单节点

# Alpha All in One 传统模式

> 这个镜像中包含 Pack 所需要的所有服务（postgres elasticsearch), 只需要启动这一个镜像即可

docker run \
  -p 8080:8080 \
  -p 8090:8090 \
  -p 5432:5432 \
  -p 9200:9200 \
  coolbeevip/servicecomb-pack-allinone:latest

# 启动 Alpha 传统模式

docker run \
  -p 8080:8080 \
  -p 8090:8090 \
  -e spring.datasource.url=jdbc:postgresql://192.168.43.78:5432/saga?useSSL=false \
  -e spring.datasource.username=saga \
  -e spring.datasource.password=password \
  coolbeevip/servicecomb-pack:latest

# 启动 Alpha 状态机模式

docker run \
  -p 8080:8080 \
  -p 8090:8090 \
  -e spring.datasource.url=jdbc:postgresql://192.168.43.78:5432/saga?useSSL=false \
  -e spring.datasource.username=saga \
  -e spring.datasource.password=password \
  -e alpha.feature.akka.enabled=true \
  -e alpha.feature.akka.transaction.repository.type=elasticsearch \
  -e spring.data.elasticsearch.cluster-name=docker-cluster \
  -e spring.data.elasticsearch.cluster-nodes=192.168.43.78:9300 \
  coolbeevip/servicecomb-pack:latest

## 集群 


## 测试

java -jar alpha-benchmark-0.6.0-SNAPSHOT-exec.jar --alpha.cluster.address=0.0.0.0:8080 --n=1000 --c=100






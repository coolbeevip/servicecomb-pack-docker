
# 快速参考

Apache ServiceComb Pack 是一个支持 TCC 和 Saga 两种模式的分布式事务中间件

* 获取帮助:

  Gitter: https://gitter.im/ServiceCombUsers/Saga

* 提出问题:

  https://github.com/apache/servicecomb-pack/issues

# 什么是 Apache ServiceComb Pack?

Apache ServiceComb Pack 用于微服务应用程序的数据一致性解决方案。 ServiceComb Pack 当前通过使用Alpha 作为事务处理协调器并使用 Omega 作为事务处理代理来提供TCC和Saga分布式事务协调解决方案。

更多信息，请访问 https://github.com/apache/servicecomb-pack

# 关于镜像

镜像分为两种类型

- All-in-One: 包含 Pack 所需要的所有服务，只需要启动一个镜像即可（适合单点试用）
- 标准: 仅包含 Pack 服务，使用时需要单独部署其他依赖服务，例如 postgres elasticsearch 等（适合集群部署）

# 如何使用镜像

###  单点部署

#### All-in-One 镜像（状态机模式）

All in One 镜像内部包含所需的所有服务，你只需要启动这一个镜像即可

```bash
docker run \
  -p 8080:8080 \
  -p 8090:8090 \
  -p 5432:5432 \
  coolbeevip/servicecomb-pack:allinone-latest
```

#### 标准镜像（状态机模式）

标准镜像只包含 Alpha 服务，您需要单独部署其他公共服务，您也可以使用 [docker-compose.yaml](https://github.com/coolbeevip/servicecomb-pack-docker/blob/master/docker-compose/docker-compose.yml) 方式启动

```bash
docker run \
  -p 8080:8080 \
  -p 8090:8090 \
  -e spring.datasource.url=jdbc:postgresql://127.0.0.1:5432/saga?useSSL=false \
  -e spring.datasource.username=saga \
  -e spring.datasource.password=password \
  -e alpha.feature.akka.enabled=true \
  -e alpha.feature.akka.transaction.repository.type=elasticsearch \
  -e spring.data.elasticsearch.cluster-name=docker-cluster \
  -e spring.data.elasticsearch.cluster-nodes=127.0.0.1:9300 \
  coolbeevip/servicecomb-pack:latest
```

#### 标准镜像（DB模式）

```bash
docker run \
  -p 8080:8080 \
  -p 8090:8090 \
  -e spring.datasource.url=jdbc:postgresql://192.168.43.78:5432/saga?useSSL=false \
  -e spring.datasource.username=saga \
  -e spring.datasource.password=password \
  coolbeevip/servicecomb-pack:latest
```

#### 测试

可以使用镜像自带的基准测试用具进行测试

```bash
docker run -it coolbeevip/servicecomb-pack:latest java -jar alpha-benchmark-0.6.0-SNAPSHOT-exec.jar --alpha.cluster.address=0.0.0.0:8080 --n=1000 --c=100
```

## 集群部署

集群部署请使用标准镜像，并需要单独部署 Postgres，Kafka，Elasticsearch




## 








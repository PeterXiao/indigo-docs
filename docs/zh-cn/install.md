# 安装预览

## Docker 预览

> 创建 `docker-compose.yml` 文件包含以下内容，执行 `docker-compose up`

```yaml
version: "3"

services:
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:6.2.2
    ports:
      - "9200:9200"
      - "9300:9300"
    environment:
      discovery.type: "single-node"
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:9200"]
      interval: 30s
      timeout: 10s
      retries: 3
  ldap-service:
    image: osixia/openldap:1.2.1
    ports:
      - "389:389"
      - "636:636"
  ldap-admin:
    image: osixia/phpldapadmin:0.7.1
    ports:
      - "1080:80"
    links:
      - ldap-service
    environment:
      PHPLDAPADMIN_HTTPS: "false"
      PHPLDAPADMIN_LDAP_HOSTS: "ldap-service"
  indigo-api:
    image: asurapro/indigo-api:latest
    ports:
      - "9000:9000"
    links:
      - elasticsearch
      - mysql
      - ldap-service
    depends_on:
      - elasticsearch
      - mysql
      - ldap-service
    restart: on-failure
    environment:
      APPLICATION_SECRET: "0><>0zv0oG>JH6>YHq4Hs=5x;ht8VB>x`_lOWo<cb309F3n`k;gy1j;i[cd;zE>u"
  indigo:
    image: asurapro/indigo:latest
    ports:
      - "80:80"
    links:
      - indigo-api
```

## 下载发布版
> WIP

## 编译源码

> WIP

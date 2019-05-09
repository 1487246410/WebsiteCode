---
title: 最新版ElasticSearch免费加密
date: 2019-05-08 17:29:16
author: schizobulia
categories: 大数据
tags: 
    - 大数据
    - ElasticSearch
    - Search-Guard
    - Kibana
---

### [Search Guard官方地址](https://search-guard.com/)

```javascript
    1. 下载Es插件：./bin/elasticsearch-plugin install -b search-guard版本号
    
    2.  bash /plugins/search-guard-7/tools/install_demo_configuration.sh
    
    3. 一直输入y(最后一个选项是是否集群模式,可以选择输入n)

    4. 启动Es并打开：https://localhost:9200/_searchguard/authinfo

    5. 全部同意,然后输入账号：admin  密码：admin

    6. bash /plugins/search-guard-7/tools/hash.sh -p 新的密码  (返回一个加密过的数据)

    7. 打开 /plugins/search-guard-7/sgconfig/sg_internal_users.yml

    8. 找到admin对应hash字段的值并将第六步生产的密码替换

    9. bash /plugins/search-guard-7/tools/sgadmin_demo.sh

    10. 重启ES测试密码是否正确

```
- [Es插件版本号查询](https://docs.search-guard.com/latest/search-guard-versions)




```javascript
    1. 下载Kibana插件：./bin/kibana-plugin install kibana插件地址
    2. 修改config中kibana.yml添加以下数据

    # Use HTTPS instead of HTTP
    elasticsearch.hosts: "https://localhost:9200"

    # Configure the Kibana internal server user
    elasticsearch.username: "kibanaserver"
    elasticsearch.password: "kibanaserver"

    # Disable SSL verification because we use self-signed demo certificates
    elasticsearch.ssl.verificationMode: none

    # Whitelist the Search Guard Multi Tenancy Header
    elasticsearch.requestHeadersWhitelist: [ "Authorization", "sgtenant" ]

    # 禁止xpack
    xpack.monitoring.enabled: false
    xpack.graph.enabled: false
    xpack.ml.enabled: false
    xpack.watcher.enabled: false
    xpack.security.enabled: false

    3. 启动kibana查看是否需要输入账号密码(Es的账号密码即可)

```
- [Kibana插件版本号查询](https://search.maven.org/search?q=a:search-guard-kibana-plugin)

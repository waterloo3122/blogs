# install elasticsearch
## via yum
`vim /etc/yum.repos.d/elasticsearch.repo`
```
[elasticsearch-7.x]
name=Elasticsearch repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
```

```
yum install elasticsearch
```

## via rpm
Download elasticsearch from elatic.co

`yum localinstall elasticsearch-7.4.2-x86_64.rpm`


# configure elasticsearch

## 1 
`vim /usr/lib/systemd/system/elasticsearch.service`

add following lines 

```
# limit memlock
LimitMEMLOCK=infinity

```

below

```
StandardOutput=journal
StandardError=inherit
```

`systemctl daemon-reload`

## 2
`vim /etc/security/limits.conf`

add following lines to the bottom of file

```
elasticsearch soft memlock unlimited
elasticsearch hard memlock unlimited
```

## 3
`vim /etc/elasticsearch/elasticsearch.yml`
Here are the contents
```
cluster.name: lcmf-dev-es
node.name: node-1
path.data: /var/lib/elasticsearch
path.logs: /var/log/elasticsearch
bootstrap.memory_lock: true
network.host: 0.0.0.0
http.port: 9200
discovery.seed_hosts: ["192.168.88.228"]
cluster.initial_master_nodes: ["node-1"]
gateway.recover_after_nodes: 3
#action.destructive_requires_name: true
```

## 4 
`systemctl start elasticsearch`

`tailf /var/log/elasticsearch/lcmf-dev-es.log`





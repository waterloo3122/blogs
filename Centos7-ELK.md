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


## add another two elasticsearch node


# install kibana

## via yum 
`cat /etc/yum.repos.d/kibana.repo`

```
[kibana-7.x]
name=Kibana repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
```

`yum install kibana`


## via rpm
`yum localinstall kibana-7.4.2-x86_64.rpm`

## configure kibana
`cat /etc/kibana/kibana.yml`

```
erver.port: 5601
server.host: "127.0.0.1"
server.name: "lcmf-dev-kibana"
elasticsearch.hosts: ["http://192.168.88.228:9200"]
```

```
systemctl start kibana
tailf /var/log/messages
```

## install nginx and configure auth for kibana
```
yum install -y nginx
systemctl enable nginx
systemctl start nginx
```
`vim /etc/nginx/nginx.conf`
comment lines below
```
    #server {
    #    listen       80 default_server;
    #    listen       [::]:80 default_server;
    #    server_name  _;
    #    root         /usr/share/nginx/html;

    #    # Load configuration files for the default server block.
    #    include /etc/nginx/default.d/*.conf;

    #    location / {
    #    }

    #    error_page 404 /404.html;
    #        location = /40x.html {
    #    }

    #    error_page 500 502 503 504 /50x.html;
    #        location = /50x.html {
    #    }
    #}

```
`nginx -t`

`vim /etc/nginx/conf.d/kibana.conf`

add following

```
server {
        listen 80;
        server_name localhost;

        location / {
                # allow 192.168.88.0/24;
                # deny all;
                auth_basic "elk auth";
                auth_basic_user_file /etc/nginx/httppasswd;
                proxy_pass http://127.0.0.1:5601;
        }
}

```

`systemctl restart nginx`

assume user is admin and password is admin

`openssl passwd -1 admin`

copy the following output into /etc/nginx/httppasswd

`$1$vkuyE8du$a4n6MD17nSEu6dLtspPX31`

`cat /etc/nginx/httppasswd`

```
admin:$1$vkuyE8du$a4n6MD17nSEu6dLtspPX31
```
`systemctl restart nginx`

## access kibana via browser
http://192.168.88.228:5601

# install redis

`yum install redis`

`vim /etc/redis.conf`

change

```
bind 127.0.0.1
```
to 

```
bind 127.0.0.1 192.168.88.118
```

change 

```
# requirepass foobared
```
to

```
requirepass your-password
```

```
systemctl enable redis
systemctl start redis
redis-cli -a your-password
```

# install logstash
## via yum

`cat /etc/yum.repos.d/logstash.repo`

```
[logstash-7.x]
name=Elastic repository for 7.x packages
baseurl=https://artifacts.elastic.co/packages/7.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
```
`yum install logstash`

## via rpm

`yum localinstall logstash-7.4.2.rpm`

## configure logstash


# install filebeat
## via yum
## via rpm



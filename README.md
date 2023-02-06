## Install RabbitMQ

```
sudo apt-get install rabbitmq-server
```

sudo rabbitmqctl cluster_status

## Duplicate erlang cookie

Copy value from first instance

```
sudo cat /var/lib/rabbitmq/.erlang.cookie
```

Then paste into other instances

```
sudo vim /var/lib/rabbitmq/.erlang.cookie
```

Then restart the rabbitmq server

```
sudo service rabbitmq-server restart
```

## Join the Cluster

```
sudo rabbitmqctl stop_app
sudo rabbitmqctl reset
sudo rabbitmqctl join_cluster rabbit@ip-172-16-0-101
sudo rabbitmqctl start_app
sudo rabbitmqctl cluster_status
```

## Enable the statistics

```
sudo rabbitmq-plugins enable rabbitmq_management
sudo rabbitmq-plugins enable rabbitmq_management
sudo rabbitmq-plugins enable rabbitmq_management
```

## Enable federation plugin

```
sudo rabbitmq-plugins enable rabbitmq_federation
sudo rabbitmq-plugins enable rabbitmq_federation
sudo rabbitmq-plugins enable rabbitmq_federation
```

## Basic Queue Mirroring

Only for instance 1

```
sudo rabbitmqctl set_policy ha-fed \
 ".*" '{"federation-upstream-set":"all", "ha-mode":"nodes", "ha-params":["rabbit@ip-172-16-0-101","rabbit@ip-172-16-0-172","rabbit@ip-172-16-0-86"]}' \
 --priority 1 \
 --apply-to queues
```

## Automatic Synchronization

Only for instance 1

```
sudo rabbitmqctl set_policy ha-fed \
 ".*" '{"federation-upstream-set":"all", "ha-sync-mode":"automatic", "ha-mode":"nodes", "ha-params":["rabbit@ip-172-16-0-101","rabbit@ip-172-16-0-172","rabbit@ip-172-16-0-86"]}' \
 --priority 1 \
 --apply-to queues
```

## Create admin user

### This adds a new user and password

```
sudo rabbitmqctl add_user username password
```

### This makes the user a administrator

```
sudo rabbitmqctl set_user_tags username administrator
```

### This sets permissions for the user

```
sudo rabbitmqctl set_permissions -p / username ".*" ".*" ".*"
```

## Source

https://www.youtube.com/watch?v=FzqjtU2x6YA

https://medium.com/aubergine-solutions/setting-up-rabbitmq-over-ec2-instances-f4dfe49f6253

## Notes

### Port declaration

https://www.rabbitmq.com/install-debian.html#ports

### Auto switch connection

https://snyk.io/advisor/npm-package/amqplib/functions/amqplib.connect
# rabbitmq-cluster-ha

# ingest-container Docker

This can be used to start spring-xd as a container for use in distributed mode with all nexus modules already installed.

# Docker Compose

Use the [docker-compose.yml](docker-compose.yml) file to start up a container and place it on the same network as services started from nexusjpl/ingest-admin. Example:

    MYSQL_PASSWORD=admin ZK_HOST_IP=10.200.10.1 KAFKA_HOST_IP=10.200.10.1 docker-compose up

`MYSQL_PASSWORD` must match the password used for the user called `xd` when the MySQL database was initialized.
`ZK_HOST_IP` must be set to a valid IP address of a zookeeper host that will be used to manage Spring XD.
`KAFKA_HOST_IP` must be set to a valid IP address of a kafka broker that will be used for the transport layer of Spring XD

# Docker Run

This container relies on 5 external services that must already be running: nexusjpl/ingest-admin, MySQL, Redis, Zookeeper, and Kafka.

To start the server use:

    docker run -it \
    -e "MYSQL_PORT_3306_TCP_ADDR=mysqldb" -e "MYSQL_PORT_3306_TCP_PORT=3306" \
    -e "MYSQL_USER=xd" -e "MYSQL_PASSWORD=admin" \
    -e "REDIS_ADDR=redis" -e "REDIS_PORT=6397" \
    -e "ZOOKEEPER_CONNECT=zkhost:2181" -e "ZOOKEEPER_XD_CHROOT=springxd" \
    -e "KAFKA_BROKERS=kafka1:9092" -e "KAFKA_ZKADDRESS=zkhost:2181/kafka"
    --add-host="zkhost:10.200.10.1" \
    --add-host="kafka1:10.200.10.1"
    --network container:ingest-admin
    --name xd-admin nexusjpl/ingest-container

This mode requires a number of Environment Variables to be defined.

#####  `MYSQL_PORT_3306_TCP_ADDR`

Address to a running MySQL service

#####  `MYSQL_PORT_3306_TCP_PORT`

Port for running MySQL service

#####  `MYSQL_USER`

Username to connnect to MySQL service

#####  `MYSQL_PASSWORD`

Password for connecting to MySQL service

#####  `ZOOKEEPER_CONNECT`

Zookeeper connect string. Can be a comma-delimmited list of host:port values.

#####  `ZOOKEEPER_XD_CHROOT`

Zookeeper root node for spring-xd

#####  `REDIS_ADDR`

Address to a running Redis service

#####  `REDIS_PORT`

Port for running Redis service

#####  `KAFKA_BROKERS`

Comma-delimmited list of host:port values which define the list of Kafka brokers used for transport.

#####  `KAFKA_ZKADDRESS`

Specifies the ZooKeeper connection string in the form hostname:port where host and port are the host and port of a ZooKeeper server.  

The server may also have a ZooKeeper chroot path as part of its ZooKeeper connection string which puts its data under some path in the global ZooKeeper namespace. If so the consumer should use the same chroot path in its connection string. For example to give a chroot path of `/chroot/path` you would give the connection string as `hostname1:port1,hostname2:port2,hostname3:port3/chroot/path`.
1) Run Zookeeper

	$ docker run -it --rm --name zookeeper -p 2181:2181 -p 2888:2888 -p 3888:3888 debezium/zookeeper

2) Run Kafka

	$ docker run -it --rm --name kafka -p 9092:9092 --link zookeeper:zookeeper debezium/kafka


3) Create Dockerfile for connect using oracle instant client with following content. Note: instantclient_19_3 should be in same directory as Dockerfile: 

	FROM debezium/connect
	 
	ENV INSTANT_CLIENT_DIR=/instant_client/
	 
	USER root
	RUN yum -y install libaio && yum clean all
	 
	USER kafka
	# Deploy Oracle client and drivers
	 
	COPY instantclient_19_3/* $INSTANT_CLIENT_DIR
	COPY instantclient_19_3/xstreams.jar /kafka/libs
	COPY instantclient_19_3/ojdbc8.jar /kafka/libs


4) Build Kafka connect image
	$ docker build -t connect .

5) Run Kafka Connect

	$ docker run -it --rm --name connect -p 8083:8083 -e GROUP_ID=1 -e CONFIG_STORAGE_TOPIC=my_connect_configs -e OFFSET_STORAGE_TOPIC=my_connect_offsets -e STATUS_STORAGE_TOPIC=my_connect_statuses --link zookeeper:zookeeper --link kafka:kafka connect:latest

6) Check if kafka connect is working (On new terminal):
	$ curl -H "Accept:application/json" localhost:8083/
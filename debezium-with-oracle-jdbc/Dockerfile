ARG DEBEZIUM_VERSION
FROM debezium/connect:$DEBEZIUM_VERSION
ENV KAFKA_CONNECT_JDBC_DIR=$KAFKA_CONNECT_PLUGINS_DIR/kafka-connect-jdbc
ENV INSTANT_CLIENT_DIR=/instant_client/
ENV LD_LIBRARY_PATH=/instant_client/
ENV DYLD_LIBRARY_PATH=/instant_client/
ENV ORACLE_HOME=/instant_client/

USER root
RUN yum -y install libaio && yum clean all

USER kafka
# Deploy Oracle client and drivers

ENV KAFKA_CONNECT_JDBC_DIR=$KAFKA_CONNECT_PLUGINS_DIR/kafka-connect-jdbc
ENV INSTANT_CLIENT_DIR=/instant_client/
ENV LD_LIBRARY_PATH=/instant_client/
ENV DYLD_LIBRARY_PATH=/instant_client/
ENV ORACLE_HOME=/instant_client/

COPY instantclient_18_5/* $INSTANT_CLIENT_DIR
COPY instantclient_18_5/xstreams.jar /kafka/libs/xstreams.jar 
COPY instantclient_18_5/ojdbc8.jar /kafka/libs/ojdbc8.jar
COPY instantclient_18_5/xstreams.jar $KAFKA_CONNECT_PLUGINS_DIR/kafka-connect-jdbc/xstreams.jar
COPY instantclient_18_5/ojdbc8.jar $KAFKA_CONNECT_PLUGINS_DIR/kafka-connect-jdbc/ojdbc8.jar

USER root

RUN chown -R kafka:kafka $KAFKA_CONNECT_PLUGINS_DIR/kafka-connect-jdbc

RUN chown -R kafka:kafka $INSTANT_CLIENT_DIR
FROM java:openjdk-8-jre

ENV DEBIAN_FRONTEND noninteractive
ENV SPARK_VERSION=2.1.1
ENV HADOOP_VERSION=2.7

ENV SPARK_HOME localhost
ENV SPARK_MASTER_PORT 7077
ENV SPARK_MASTER_HOST localhost
ENV SPARK_MASTER_WEBUI_PORT 8088
ENV SPARK_MASTER_LOG /spark/logs
ENV SPARK_MASTER_NAME spark-master
ENV SPARK_APPLICATION_JAR_LOCATION /app/application.jar
ENV SPARK_APPLICATION_MAIN_CLASS my.main.Application
ENV SPARK_APPLICATION_ARGS ""

ENV SPARK_MASTER_URL spark://${SPARK_MASTER_NAME}:${SPARK_MASTER_PORT}
ENV SPARK_HOME /opt/apache-spark

#ENV BIGTOP_VERSION=1.2.0
#ADD https://www.apache.org/dist/bigtop/bigtop-${BIGTOP_VERSION}/repos/debian8/bigtop.list /etc/apt/sources.list.d/
#RUN curl https://www.apache.org/dist/bigtop/bigtop-${BIGTOP_VERSION}/repos/GPG-KEY-bigtop | apt-key add -
#RUN apt-get update && \
#    apt-get install -y spark-core spark-master supervisor && \
#    rm -rf /var/lib/apt/lists/* && \
#    apt-get clean
    
RUN apt-get update && \
    apt-get install -y wget supervisor && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get clean && \
    wget -q http://apache.proserve.nl/spark/spark-"$SPARK_VERSION"/spark-"$SPARK_VERSION"-bin-hadoop"$HADOOP_VERSION".tgz \
    -O /tmp/spark-"$SPARK_VERSION"-bin-hadoop"$HADOOP_VERSION".tgz && \
    mkdir "$SPARK_HOME" && \
    tar zxvf /tmp/spark-"$SPARK_VERSION"-bin-hadoop"$HADOOP_VERSION".tgz -C "$SPARK_HOME" --strip 1 && \
    rm /tmp/spark-"$SPARK_VERSION"-bin-hadoop"$HADOOP_VERSION".tgz
    

ADD spark-supervisor.conf /etc/supervisor/conf.d/
ADD start-spark-master.sh "$SPARK_HOME"/sbin/start-spark-master.sh
RUN chmod 755 "$SPARK_HOME"/sbin/start-spark-master.sh

EXPOSE 8088 7077
CMD ["supervisord", "-n"]
#CMD ["sh", "/opt/apache-spark/sbin/start-spark-master.sh"]


#RUN echo "Submit application ${SPARK_APPLICATION_JAR_LOCATION} with main class ${SPARK_APPLICATION_MAIN_CLASS} to Spark master ${SPARK_MASTER_URL}"
#RUN echo "Passing arguments ${SPARK_APPLICATION_ARGS}"
#CMD /spark/bin/spark-submit \
#        --class ${SPARK_APPLICATION_MAIN_CLASS} \
#       --master ${SPARK_MASTER_URL} \
#        ${SPARK_APPLICATION_JAR_LOCATION} ${SPARK_APPLICATION_ARGS}

#RUN mkdir "$FLUME_HOME"/logs
#ADD conf/flume.conf "$FLUME_CONF_FILE"
#ADD conf/log4j.properties "$FLUME_CONF_DIR"/log4j.properties

#ADD run/flume-supervisor.conf /etc/supervisor/conf.d/
#ADD run/start-flume.sh "$FLUME_HOME"/bin/start-flume.sh
#RUN chmod 755 "$FLUME_HOME"/bin/start-flume.sh

#EXPOSE 1234
#CMD ["supervisord", "-n"]
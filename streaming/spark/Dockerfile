# FROM uhopper/hadoop:2.7.2
FROM hseeberger/scala-sbt:8u151-2.12.4-1.0.4
VOLUME /tmp

ENV SPARK_VERSION spark-2.2.1-bin-hadoop2.7
ENV SPARK_HOME /usr/local/spark

RUN curl http://apache.mediamirrors.org/spark/spark-2.2.1/$SPARK_VERSION.tgz -o $SPARK_VERSION.tgz; \
                 tar xzf $SPARK_VERSION.tgz -C /usr/local/;
RUN cd /usr/local && ln -s $SPARK_VERSION spark

ADD hbase-site.xml $SPARK_HOME/conf

ENV PROJECT_HOME /usr/local/project

ADD src $PROJECT_HOME/src
ADD build.sbt $PROJECT_HOME/
ADD project/build.properties $PROJECT_HOME/project/
ADD project/plugins.sbt $PROJECT_HOME/project/

WORKDIR $PROJECT_HOME

RUN sbt package assembly

WORKDIR $SPARK_HOME

CMD ["bin/spark-submit", "--class", "com.mitosis.Main", "--master", "yarn", "--deploy-mode", "cluster", "/usr/local/project/target/scala-2.11/search-flight-streaming-spark-assembly-0.1.0.jar" ]

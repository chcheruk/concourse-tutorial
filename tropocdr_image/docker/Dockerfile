FROM alpine:3.4

ENV LOG4J_VER 1.2.17
ENV LOG4J_BIN log4j-${LOG4J_VER}.tar.gz

ENV JAVA_HOME /usr/bin

USER root

RUN apk add --no-cache curl wget ca-certificates openjdk8-jre-base bash


RUN wget -O /tmp/${LOG4J_BIN} http://apache-mirror.rbc.ru/pub/apache/logging/log4j/${LOG4J_VER}/${LOG4J_BIN} && \
    cd /tmp/ && tar -xzf ${LOG4J_BIN} && mkdir -p /opt/log4j && mv apache-log4j-${LOG4J_VER}/log4j-${LOG4J_VER}.jar /opt/log4j/ && \
    rm -rf apache-log4j-${LOG4J_VER} ${LOG4J_BIN}

WORKDIR /opt/
ENV CDR_VERSION trunk-SNAPSHOT

RUN curl -s http://artifacts.voxeolabs.net/tropo/${CDR_VERSION}/artifacts/cdr/tropo-cdr-${CDR_VERSION}-dist.tar.gz | tar zxv

RUN mv /opt/tropo-pause /opt/tropo-cdr-production
ADD http://10.203.54.42:9000/robotresults/run_cdr.sh /run/run_cdr.sh
RUN chmod +x /run/run_cdr.sh

ADD http://10.203.54.42:9000/robotresults/config/billwise-rest.properties /opt/tropo-cdr-production/config/
ADD http://10.203.54.42:9000/robotresults/config/billwise.properties /opt/tropo-cdr-production/config/
ADD http://10.203.54.42:9000/robotresults/config/bridge.properties /opt/tropo-cdr-production/config/
ADD http://10.203.54.42:9000/robotresults/config/databaseSms.properties /opt/tropo-cdr-production/config/
ADD http://10.203.54.42:9000/robotresults/config/file.properties /opt/tropo-cdr-production/config/
ADD http://10.203.54.42:9000/robotresults/config/jms.properties /opt/tropo-cdr-production/config/
ADD http://10.203.54.42:9000/robotresults/config/log4j.xml /opt/tropo-cdr-production/config/
ADD http://10.203.54.42:9000/robotresults/config/kafkaCDRConsumer.properties /opt/tropo-cdr-production/config/
ADD http://10.203.54.42:9000/robotresults/config/kafkaDbConsumer.properties /opt/tropo-cdr-production/config/
#ADD http://10.203.54.42:9000/robotresults/config/database.properties /opt/tropo-cdr-production/config/
ADD http://10.203.54.42:9000/robotresults/config/META-INF/jms-context.xml /opt/tropo-cdr-production/config/META-INF/jms-context.xml

ADD http://10.203.54.42:9000/robotresults/mysql-connector-java-5.1.23.jar /opt/tropo-cdr-production/lib/

RUN apk add --no-cache ruby ruby-bundler ruby-dev build-base
RUN gem install json tiller --no-doc
#ADD http://10.203.54.42:9000/robotresults/data/tiller/common.yaml /etc/tiller
#ADD http://10.203.54.42:9000/robotresults/data/tiller/templates/database.properties.erb /etc/tiller

RUN apk add --no-cache ruby ruby-bundler ruby-dev build-base
RUN gem install json tiller --no-doc
ADD data/tiller /etc/tiller

#
# Tiller is the support package that will take environmental variables and inject them
# into the configuration templates at startup.  It will then run the java commandline
# to startup the Tropo CDR java executible.
#

CMD [ "/usr/bin/tiller", "-v","-d"]



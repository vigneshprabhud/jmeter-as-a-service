FROM alpine

MAINTAINER Vignesh Prabhu <vignesh.prabhu@outlook.com>

USER root

ARG JMETER_VERSION="3.2"
ARG JAVA_VERSION="openjdk8"
ARG JMETER_REPO="https://archive.apache.org/dist/jmeter/binaries"
ARG LOGSTASH_VERSION="6.0.0"

RUN \
  apk add --update \
  && apk add --no-cache ${JAVA_VERSION} wget bash libc6-compat \
  && rm -rf /var/cache/apk/* \
  && cd /tmp \
	&& wget https://artifacts.elastic.co/downloads/logstash/logstash-${LOGSTASH_VERSION}.tar.gz \
	&& tar -xzf logstash-${LOGSTASH_VERSION}.tar.gz \
  && rm logstash-${LOGSTASH_VERSION}.tar.gz \
  && mv logstash-${LOGSTASH_VERSION} /usr/share/logstash \
  && rm -rf /tmp/*

COPY jmeter_to_elastic.logstash /usr/share/logstash/

ENV JAVA_HOME /usr/lib/jvm/java-1.8-openjdk
ENV LOGSTASH_HOME /usr/share/logstash

ENV PATH $JAVA_HOME/bin:$PATH
ENV PATH $LOGSTASH_HOME/bin:$PATH

CMD ["logstash","-f","/usr/share/logstash/jmeter_to_elastic.logstash"]

#TO BUILD
#docker build -t disney-logstash -f Dockerfile_logstash .

#TO START LOGSTASH
#docker run -v /home/vignesh/jmeter/results/:/jmeter/results/ --net=host -dit --name disney-logstash disney-logstash

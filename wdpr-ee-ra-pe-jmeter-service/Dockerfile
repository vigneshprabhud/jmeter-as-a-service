FROM alpine

MAINTAINER Vignesh Prabhu <vignesh.prabhu@outlook.com>

USER root

ARG JMETER_VERSION="3.2"
ARG JAVA_VERSION="openjdk8"
ARG JMETER_REPO="https://archive.apache.org/dist/jmeter/binaries"

RUN \
  apk add --update \
  && apk add --no-cache ${JAVA_VERSION} wget \
  && rm -rf /var/cache/apk/* \
  && mkdir -p /jmeter/scripts /jmeter/results \
	&& cd /jmeter/ \
	&& wget ${JMETER_REPO}/apache-jmeter-${JMETER_VERSION}.tgz \
	&& tar -xzf apache-jmeter-${JMETER_VERSION}.tgz \
  && rm apache-jmeter-${JMETER_VERSION}.tgz

COPY src/*.jmx /jmeter/scripts/

ENV JAVA_HOME /usr/lib/jvm/java-1.8-openjdk
ENV JMETER_HOME /jmeter/apache-jmeter-${JMETER_VERSION}
ENV PATH $JAVA_HOME/bin:$PATH
ENV PATH $JMETER_HOME/bin:$PATH

CMD ["sh","-c","jmeter -n -t /jmeter/scripts/*.jmx -l /jmeter/results/results.jtl -JapplicationName=${APPLICATION_NAME} -JtestId=${TEST_ID} -JtestDesc=${TEST_DESC} -JapplicationVersion=${APPLICATION_VERSION} -Jjmeter.save.saveservice.print_field_names=false -Jjmeter.save.saveservice.autoflush=true -Jsample_variables=conversationId,applicationName,testId,testDesc,applicationVersion -Jjmeter.save.saveservice.response_data.on_error=true"]

#TO BUILD
#docker build -t disney-jmeter .

#TO START TEST
#docker run -v /home/vignesh/jmeter/results/:/jmeter/results -e APPLICATION_NAME=DCL -e TEST_ID=20171209 -e TEST_DESC=AnotherTest -e APPLICATION_VERSION=4.5 --net=host -dit --name disney-jmeter disney-jmeter

#TO STOP TEST
#docker exec -d disney-jmeter java -cp /jmeter/apache-jmeter-3.2/bin/ApacheJMeter.jar org.apache.jmeter.util.ShutdownClient Shutdown

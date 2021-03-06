FROM alpine:latest

# Env Vars
ENV LANG=en_US.UTF-8
ENV JAVA_TOOL_OPTIONS=-Dfile.encoding=UTF8

WORKDIR /loklak_server

RUN apk update && apk add openjdk8 git bash && \
    git clone https://github.com/loklak/loklak_server.git /loklak_server && \
    git checkout origin/master && \
    ./gradlew build -x test -x checkstyleTest -x checkstyleMain -x jacocoTestReport && \
    sed -i.bak 's/^\(port.http=\).*/\180/' conf/config.properties && \
    sed -i.bak 's/^\(port.https=\).*/\1443/' conf/config.properties && \
    sed -i.bak 's/^\(upgradeInterval=\).*/\186400000000/' conf/config.properties && \
    sed -i.bak 's/^\(elasticsearch_transport.enabled\).*/\1=true/' conf/config.properties && \
    sed -i.bak 's/^\(elasticsearch_transport.addresses\).*/\1=elasticsearch.elasticsearch:9300/' conf/config.properties && \
    sed -i.bak 's/^\(dump.write_enabled\).*/\1=false/' conf/config.properties && \
    sed -i.bak 's/^\(backend=\).*/\1http:\/\/root.loklak.org/' conf/config.properties && \
    echo "while true; do sleep 10;done" >> bin/start.sh

# Start
CMD ["bin/start.sh", "-Idn"]

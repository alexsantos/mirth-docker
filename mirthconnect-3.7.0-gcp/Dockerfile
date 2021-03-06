FROM openjdk:11-jre-slim

ENV MIRTH_CONNECT_VERSION 3.7.0.b2399
ENV MIRTH_HOME /opt/Mirth\ Connect
ENV FHIR_VERSION 3.7.0.b1046

# Mirth Connect is run with user `connect`, uid = 1000
# If you bind mount a volume from the host or a data container, 
# ensure you use the same uid
RUN useradd -u 1000 mirth

# grab gosu for easy step-down from root
RUN apt-get update && apt-get install -y --no-install-recommends dirmngr gnupg ca-certificates wget \
	&& rm -rf /var/lib/apt/lists/* \
	&& mkdir ~/.gnupg && echo "disable-ipv6" >> ~/.gnupg/dirmngr.conf \
	&& gpg --batch --keyserver hkp://pool.sks-keyservers.net:80 --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4 \
	&& wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/1.2/gosu-$(dpkg --print-architecture)" \
	&& wget -O /usr/local/bin/gosu.asc "https://github.com/tianon/gosu/releases/download/1.2/gosu-$(dpkg --print-architecture).asc" \
	&& gpg --verify /usr/local/bin/gosu.asc \
	&& rm /usr/local/bin/gosu.asc \
	&& chmod +x /usr/local/bin/gosu \
	&& wget https://dl.google.com/cloudsql/cloud_sql_proxy.linux.amd64 -O /usr/local/bin/cloud_sql_proxy \
	&& chmod +x /usr/local/bin/cloud_sql_proxy

ADD mirthconnect-$MIRTH_CONNECT_VERSION-unix.tar.gz /opt/
ADD fhir-$FHIR_VERSION.tar.gz "$MIRTH_HOME"/extensions/

RUN chown -R mirth /opt/Mirth\ Connect

WORKDIR /opt/Mirth\ Connect

EXPOSE 8080 8443

COPY docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]

CMD /usr/local/bin/cloud_sql_proxy -instances=$INSTANCE_CONNECTION_NAME=tcp:5432 & ./mcserver

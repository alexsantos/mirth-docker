FROM openjdk:8-jre

ENV MIRTH_CONNECT_VERSION 3.6.1.b220
ENV MIRTH_HOME /opt/Mirth\ Connect

# Mirth Connect is run with user `connect`, uid = 1000
# If you bind mount a volume from the host or a data container, 
# ensure you use the same uid
RUN useradd -u 1000 mirth

# grab gosu for easy step-down from root
RUN gpg --keyserver hkp://pool.sks-keyservers.net:80 --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4
RUN apt-get update && apt-get install -y --no-install-recommends ca-certificates wget && rm -rf /var/lib/apt/lists/* \
	&& wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/1.2/gosu-$(dpkg --print-architecture)" \
	&& wget -O /usr/local/bin/gosu.asc "https://github.com/tianon/gosu/releases/download/1.2/gosu-$(dpkg --print-architecture).asc" \
	&& gpg --verify /usr/local/bin/gosu.asc \
	&& rm /usr/local/bin/gosu.asc \
	&& chmod +x /usr/local/bin/gosu

ADD mirthconnect-$MIRTH_CONNECT_VERSION-unix.tar.gz /opt/

RUN chown -R mirth /opt/Mirth\ Connect

WORKDIR /opt/Mirth\ Connect

EXPOSE 8080 8443

COPY docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]

CMD [ "./mcserver" ]

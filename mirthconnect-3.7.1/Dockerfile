FROM openjdk:11-jre-slim

ENV MIRTH_CONNECT_VERSION 3.7.1.b243
ENV MIRTH_HOME /opt/Mirth\ Connect
ENV FHIR_VERSION 3.7.1.b258

# Mirth Connect is run with user `connect`, uid = 500
# If you bind mount a volume from the host or a data container, 
# ensure you use the same uid
RUN useradd -u 500 mirth

# grab gosu for easy step-down from root
RUN apt-get update && apt-get install -y --no-install-recommends dirmngr gnupg ca-certificates wget \
	&& rm -rf /var/lib/apt/lists/* \
	&& mkdir ~/.gnupg && chmod 700 ~/.gnupg && echo "disable-ipv6" >> ~/.gnupg/dirmngr.conf \
	&& gpg --batch --keyserver hkp://pool.sks-keyservers.net:80 --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4 \
	&& wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/1.2/gosu-$(dpkg --print-architecture)" \
	&& wget -O /usr/local/bin/gosu.asc "https://github.com/tianon/gosu/releases/download/1.2/gosu-$(dpkg --print-architecture).asc" \
	&& gpg --verify /usr/local/bin/gosu.asc \
	&& rm /usr/local/bin/gosu.asc \
	&& chmod +x /usr/local/bin/gosu

ADD mirthconnect-$MIRTH_CONNECT_VERSION-unix.tar.gz /opt/
ADD fhir-$FHIR_VERSION.tar.gz "$MIRTH_HOME"/extensions/

RUN sed -i 's/Xmx256m/Xmx512m/g' "${MIRTH_HOME}"/mcserver.vmoptions \
    && sed -i 's/Xmx256m/Xmx512m/g' "${MIRTH_HOME}"/mcservice.vmoptions \
	&& echo "-Duser.timezone=UTC" >> "${MIRTH_HOME}"/mcserver.vmoptions \
    && echo "-Duser.timezone=UTC" >> "${MIRTH_HOME}"/mcservice.vmoptions \
	&& cat "${MIRTH_HOME}"/docs/mcservice-java9+.vmoptions >> "${MIRTH_HOME}"/mcserver.vmoptions \
    && cat "${MIRTH_HOME}"/docs/mcservice-java9+.vmoptions >> "${MIRTH_HOME}"/mcservice.vmoptions

RUN chown -R mirth.mirth /opt/Mirth\ Connect

WORKDIR /opt/Mirth\ Connect

EXPOSE 8080 8443

COPY docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]

USER mirth

CMD ./mcserver

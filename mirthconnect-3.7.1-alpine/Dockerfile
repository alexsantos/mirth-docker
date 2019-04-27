FROM openjdk:8-jre-alpine

ENV MIRTH_CONNECT_VERSION 3.7.1.b243
ENV MIRTH_HOME /opt/Mirth\ Connect
ENV FHIR_VERSION 3.7.1.b258

# Mirth Connect is run with user `connect`, uid = 500
# If you bind mount a volume from the host or a data container, 
# ensure you use the same uid
RUN addgroup --gid 500 mirth && adduser -D -H -u 500 -G mirth mirth

ADD mirthconnect-$MIRTH_CONNECT_VERSION-unix.tar.gz /opt/
ADD fhir-$FHIR_VERSION.tar.gz "$MIRTH_HOME"/extensions/

RUN sed -i 's/Xmx256m/Xmx512m/g' "${MIRTH_HOME}"/mcserver.vmoptions \
    && sed -i 's/Xmx256m/Xmx512m/g' "${MIRTH_HOME}"/mcservice.vmoptions \
	&& echo "-Duser.timezone=UTC" >> "${MIRTH_HOME}"/mcserver.vmoptions \
    && echo "-Duser.timezone=UTC" >> "${MIRTH_HOME}"/mcservice.vmoptions

RUN chown -R mirth.mirth /opt/Mirth\ Connect

WORKDIR /opt/Mirth\ Connect

EXPOSE 8080 8443

USER mirth

CMD ./mcserver

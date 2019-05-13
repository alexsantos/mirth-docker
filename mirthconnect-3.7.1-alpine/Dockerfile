FROM openjdk:8-jre-alpine

ENV MIRTH_CONNECT_VERSION 3.7.1.b243
ENV MIRTH_HOME /opt/Mirth\ Connect

# Mirth Connect is run with user `connect`, uid = 500
# If you bind mount a volume from the host or a data container, 
# ensure you use the same uid
RUN addgroup --gid 500 mirth && adduser -D -H -u 500 -G mirth mirth

RUN mkdir -p "${MIRTH_HOME}" \
    && chown -R mirth.mirth "${MIRTH_HOME}"
USER mirth
COPY mirthconnect-${MIRTH_CONNECT_VERSION}-unix.tar.gz "${MIRTH_HOME}"
RUN tar xvf "${MIRTH_HOME}"/mirthconnect-${MIRTH_CONNECT_VERSION}-unix.tar.gz --strip-components=1 -C "${MIRTH_HOME}" \
    && rm "${MIRTH_HOME}"/mirthconnect-${MIRTH_CONNECT_VERSION}-unix.tar.gz

EXPOSE 8080 8443

CMD "${MIRTH_HOME}"/mcserver

FROM centos:centos7
MAINTAINER Erik Nord <erik.nord@akana.com>

ENV AKANA_ROOT /opt/akana_sw
ENV TMP_STAGE ${AKANA_ROOT}/stage
ENV TMP_RESOURCES ${TMP_STAGE}/resources
ENV TMP_INSTALL ${TMP_STAGE}/install
ENV INSTALL_LOGS ${AKANA_ROOT}/logs
ENV CONTAINER_LOGS /var/log/akana_sw
ENV LOG_VIEWER ${AKANA_ROOT}/logviewer

RUN yum -y update \
    && curl --silent --location https://rpm.nodesource.com/setup_4.x | bash - \
    && yum -y install unzip make gcc net-tools openscap-scanner scap-security-guide prelink openssl \
    && groupadd -r akanaAdmin && useradd -r -g akanaAdmin akanaAdmin \
    && mkdir -p ${AKANA_ROOT} \
        ${TMP_STAGE} \
        ${TMP_RESOURCES} \
        ${TMP_INSTALL} \
        ${INSTALL_LOGS} \
        ${TMP_STAGE}/certs \
        ${CONTAINER_LOGS} \
        ${LOG_VIEWER} \
    && curl -L "https://gitlab.com/akana/logviewer/builds/artifacts/master/download?job=build_with_gb" -H "PRIVATE-TOKEN: hmVXP5wSCwUNzvxd-L46" -o ${LOG_VIEWER}/artifacts.zip \
    && unzip -o ${LOG_VIEWER}/artifacts.zip bin/logviewer-linux-amd64 -d ${LOG_VIEWER} \
    && mv ${LOG_VIEWER}/bin/logviewer-linux-amd64 ${LOG_VIEWER}/logviewer \
    && rm -rf ${LOG_VIEWER}/bin ${LOG_VIEWER}/artifacts.zip

ADD ./files/resources/* /${TMP_RESOURCES}/

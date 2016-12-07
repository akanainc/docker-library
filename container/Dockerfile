FROM akanainc/base:8.2.8
MAINTAINER Erik Nord <erik.nord@akana.com>

#ENV AUTOMATION ps-automation.zip

# Bring in all of the automation scripting
ADD ./files/install/* /${TMP_INSTALL}/
ADD ./installer.properties /${TMP_INSTALL}/properties/installer.properties

RUN curl -L "https://gitlab.com/akana/automation/builds/artifacts/8.2.4/download?job=automation-job" -H "PRIVATE-TOKEN: hmVXP5wSCwUNzvxd-L46" -o ${TMP_INSTALL}/ps-automation.zip \
    && unzip -o ${TMP_INSTALL}/ps-automation.zip -x */installer.properties -d ${TMP_INSTALL} \
	&& ${TMP_INSTALL}/installer.py -i -s --javaHome=/opt/akana_sw/sm8/jre --installerlog DEBUG | tee ${INSTALL_LOGS}/containerInstall.log \
	&& rm -rf ${TMP_INSTALL}/ps-automation.zip ${TMP_RESOURCES}/*.zip ${AKANA_ROOT}/sm8/docs ${AKANA_ROOT}/sm8/dbscripts ${AKANA_ROOT}/sm8/samples ${AKANA_ROOT}/sm8/api-portal-dbscripts \
 	&& chmod 755 -R ${AKANA_ROOT} \
 	&& chown -R akanaAdmin:akanaAdmin ${AKANA_ROOT} \
 	&& chmod 755 -R ${CONTAINER_LOGS} \
 	&& chown -R akanaAdmin:akanaAdmin ${CONTAINER_LOGS}

USER akanaAdmin

#EXPOSE 9900 10001

ENTRYPOINT ["/opt/akana_sw/stage/install/createDockerContainer.py"]

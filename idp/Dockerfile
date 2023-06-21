FROM unicon/shibboleth-idp:latest

# There's an exception otherwise; not interested in metrics anyways
RUN sed -i '/bean id="shibboleth.metrics.MetadataGaugeSet"/,+4d' /opt/shibboleth-idp/system/conf/general-admin-system.xml \
    && sed -i '/shibboleth.metrics.MetadataGaugeSet/d' /opt/shibboleth-idp/conf/admin/metrics.xml
COPY shib-jetty-base/ /opt/shib-jetty-base/

RUN curl -sLo /opt/shib-jetty-base/lib/ext/commons-pool2-2.11.1.jar https://search.maven.org/remotecontent?filepath=org/apache/commons/commons-pool2/2.11.1/commons-pool2-2.11.1.jar \
 && curl -sLo /opt/shib-jetty-base/lib/ext/commons-dbcp2-2.9.0.jar https://search.maven.org/remotecontent?filepath=org/apache/commons/commons-dbcp2/2.9.0/commons-dbcp2-2.9.0.jar \
 && curl -sLo /opt/shib-jetty-base/lib/ext/commons-logging-api-1.1.jar https://repo1.maven.org/maven2/commons-logging/commons-logging-api/1.1/commons-logging-api-1.1.jar \
 && curl -sLo /tmp/mysql-connector-java-5.1.49.tar.gz http://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.49.tar.gz \
 && tar -C /tmp -xvf /tmp/mysql-connector-java-5.1.49.tar.gz && cp /tmp/mysql-connector-java-5.1.49/mysql-connector-java-5.1.49-bin.jar /opt/shib-jetty-base/lib/ext/ && rm -rf /tmp/mysql-connector*

COPY shibboleth-idp/ /opt/shibboleth-idp/

ARG HOSTNAME_FOR_BROWSER 
RUN sed -i "s#Location=\"https://idptestbed#Location=\"https://${HOSTNAME_FOR_BROWSER}#" /opt/shibboleth-idp/metadata/idp-metadata.xml \
    && sed -i "s#Location=\"https://idptestbed#Location=\"https://${HOSTNAME_FOR_BROWSER}#" /opt/shibboleth-idp/metadata/sp-metadata.xml

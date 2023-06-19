FROM unicon/shibboleth-idp:latest

MAINTAINER Unicon, Inc.

# There's an exception otherwise; not interested in metrics anyways
RUN sed -i '/bean id="shibboleth.metrics.MetadataGaugeSet"/,+4d' /opt/shibboleth-idp/system/conf/general-admin-system.xml \
    && sed -i '/shibboleth.metrics.MetadataGaugeSet/d' /opt/shibboleth-idp/conf/admin/metrics.xml
COPY shibboleth-idp/ /opt/shibboleth-idp/
COPY shib-jetty-base/ /opt/shib-jetty-base/

ARG HOSTNAME_FOR_BROWSER 
RUN sed -i "s#Location=\"https://idptestbed#Location=\"https://${HOSTNAME_FOR_BROWSER}#" /opt/shibboleth-idp/metadata/idp-metadata.xml \
    && sed -i "s#Location=\"https://idptestbed#Location=\"https://${HOSTNAME_FOR_BROWSER}#" /opt/shibboleth-idp/metadata/sp-metadata.xml

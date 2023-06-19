FROM centos:centos7

# https://github.com/ConsortiumGARR/idem-tutorials/blob/master/idem-fedops/HOWTO-Shibboleth/Service%20Provider/CentOS/HOWTO%20Install%20and%20Configure%20a%20Shibboleth%20SP%20v3.x%20on%20CentOS%207%20(x86_64).md

COPY etc-yum/shibboleth.repo /etc/yum.repos.d/shibboleth.repo

RUN yum -y update \
    && yum -y install httpd php mod_ssl shibboleth   \
    && yum -y clean all

RUN mv /etc/httpd/conf.d/welcome.conf /etc/httpd/conf.d/welcome.conf.deactivated \ 
    && sed -i "s/Options Indexes FollowSymLinks/Options FollowSymLinks/" /etc/httpd/conf/httpd.conf

COPY etc-shibboleth /etc/shibboleth/
COPY etc-httpd/ /etc/httpd/
COPY var-www-html/ /var/www/html/

COPY httpd-and-shibd /usr/local/bin/

RUN chmod +x /usr/local/bin/httpd-and-shibd

ARG HOSTNAME_FOR_BROWSER 
RUN sed -i "s#Location=\"https://idptestbed#Location=\"https://${HOSTNAME_FOR_BROWSER}#" /etc/shibboleth/idp-metadata.xml \
    && sed -i "s#idptestbed#${HOSTNAME_FOR_BROWSER}#g" /etc/httpd/conf.d/sp.conf

EXPOSE 80

CMD ["httpd-and-shibd"]

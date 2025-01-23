FROM centos:centos7

RUN sed -i -e '/^mirrorlist/d' -e 's/^#baseurl/baseurl/' -e 's#mirror#vault#' /etc/yum.repos.d/CentOS-Base.repo \
    && yum -y update \
    && yum -y install httpd mod_ssl mod_ldap \
    && yum -y clean all

COPY httpd-foreground /usr/local/bin/
COPY etc-httpd/ /etc/httpd/
COPY var-www-html/ /var/www/html/

RUN chmod +x /usr/local/bin/httpd-foreground

EXPOSE 80 443

CMD ["httpd-foreground"]
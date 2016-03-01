FROM dockerimages/ubuntu-core:14.04
MAINTAINER Frank Lemanschik @ B8G Datentechnik


RUN echo deb http://us.archive.ubuntu.com/ubuntu precise main universe >> /etc/apt/sources.list
RUN apt-get -y update && \
    apt-get -y -q --no-install-recommends install \
    apache2  \
    libcurl3 \
    libhiredis0.10 \
    libjansson4 \
    apache2-utils \
    nano \
    curl \
    wget && \
    apt-get -y clean 

RUN wget https://github.com/pingidentity/mod_auth_openidc/releases/download/v1.8.4/libapache2-mod-auth-openidc_1.8.4-1.bpo70.1_amd64.deb --no-check-certificate 
RUN dpkg -i libapache2-mod-auth-openidc_1.8.4-1.bpo70.1_amd64.deb && rm libapache2-mod-auth-openidc_1.8.4-1.bpo70.1_amd64.deb

#RUN a2enmod auth_openidc

#RUN a2enmod proxy
#RUN a2enmod proxy_http
#RUN a2enmod proxy_wstunnel

#RUN htpasswd -c  -b /etc/apache2/htpass test test
RUN htpasswd -c -b /etc/apache2/htpass test1 test1
RUN htpasswd -b /etc/apache2/htpass test0 test0

#RUN rm etc/apache2/sites-enabled/000-default.conf
#COPY  000-default.conf  etc/apache2/sites-enabled/000-default.conf


# let's copy a few of the settings from /etc/init.d/apache2
ENV VIRTUAL_DOMAIN examplle.com
ENV VIRTUAL_PORT 80
ENV DOCKER_RUN docker run -d -e VIRTUAL_DOMAIN=example.com -v /var/www/${1}:/var/www/html dockerimages/apache2
ENV DOCKER_BUILD docker build -t dockerimages/apache2 git://github.com/dockerimages/apache2
ENV APACHE_CONFDIR /etc/apache2
ENV APACHE_ENVVARS $APACHE_CONFDIR/envvars
# and then a few more from $APACHE_CONFDIR/envvars itself
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_RUN_DIR /var/run/apache2
ENV APACHE_PID_FILE $APACHE_RUN_DIR/apache2.pid
ENV APACHE_LOCK_DIR /var/lock/apache2
ENV APACHE_LOG_DIR /var/log/apache2
ENV LANG C

# ...
RUN mkdir -p $APACHE_RUN_DIR $APACHE_LOCK_DIR $APACHE_LOG_DIR

# make CustomLog (access log) go to stdout instead of files
#  and ErrorLog to stderr
RUN find "$APACHE_CONFDIR" -type f -exec sed -ri ' \
	s!^(\s*CustomLog)\s+\S+!\1 /proc/self/fd/1!g; \
	s!^(\s*ErrorLog)\s+\S+!\1 /proc/self/fd/2!g; \
' '{}' ';'

RUN mkdir /var/www/shiny0
RUN mkdir /var/www/shiny1
RUN mv /var/www/html/index.html /var/www/index2.html
COPY index.html /var/www/html/index.html

#RUN a2enmod auth_openidc
RUN a2enmod proxy
RUN a2enmod proxy_http
RUN a2enmod proxy_wstunnel

#RUN  rm /etc/apache2/mods-enabled/auth_openidc.load
RUN  rm etc/apache2/sites-enabled/000-default.conf
COPY  000-default.conf  etc/apache2/sites-enabled/000-default.conf



EXPOSE 80

CMD ["apache2", "-DFOREGROUND"]

#CMD ["R"]
#bash", "-DFOREGROUND"]

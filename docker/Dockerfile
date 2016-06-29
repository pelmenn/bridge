FROM ubuntu:16.04

WORKDIR /tmp

RUN apt-get update

#install other tools
RUN apt-get install -y \
	curl \
	wget \
	sudo \
	mc

#install nginx
#ADD resources/nginx/nginx_signing.key /tmp/nginx_signing.key
#RUN apt-key add nginx_signing.key \
#	&& rm /tmp/nginx_signing.key \
#	&& echo "deb http://nginx.org/packages/ubuntu/ xenial nginx" >> /etc/apt/sources.list.d/nginx.list \
#	&& echo "deb-src http://nginx.org/packages/ubuntu/ xenial nginx" >> /etc/apt/sources.list.d/nginx.list \
#	&& apt-get update \
#	&& apt-get install -y nginx
#ADD resources/nginx/nginx.conf /etc/nginx/nginx.conf

#install postgresql
#ADD resources/postgresql/ACCC4CF8.asc /tmp/ACCC4CF8.asc
#RUN apt-key add ACCC4CF8.asc \
#	&& rm /tmp/ACCC4CF8.asc \
#	&& echo "deb http://apt.postgresql.org/pub/repos/apt/ xenial-pgdg main" >> /etc/apt/sources.list.d/postgresql.list \
#	&& apt-get update \
#	&& apt-get install -y postgresql-9.4

#install php7
RUN apt-get update \
	&& apt-get install -y php7.0 php7.0-xml
#ADD resources/php/nginx.conf /etc/nginx/nginx.conf

#install composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "if (hash_file('SHA384', 'composer-setup.php') === 'e115a8dc7871f15d853148a7fbac7da27d6c0030b848d9b3dc09e2a0388afed865e6a3d6b3c0fad45c48e2b5fc1196ae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php --install-dir=/bin \
    && php -r "unlink('composer-setup.php');"

#create user system
RUN groupadd -f -g 1000 user \
	&& useradd -m -g user -u 1000 user
FROM ubuntu:16.04

WORKDIR /tmp

RUN apt-get update

#install tools
RUN apt-get install -y \
	curl \
	wget \
	mc

#install nginx
ADD resources/nginx/nginx_signing.key /tmp/nginx_signing.key
RUN apt-key add nginx_signing.key \
	&& rm /tmp/nginx_signing.key \
	&& echo "deb http://nginx.org/packages/ubuntu/ xenial nginx" >> /etc/apt/sources.list.d/nginx.list \
	&& echo "deb-src http://nginx.org/packages/ubuntu/ xenial nginx" >> /etc/apt/sources.list.d/nginx.list \
	&& apt-get update \
	&& apt-get install -y nginx
ADD resources/nginx/nginx.conf /etc/nginx/nginx.conf

#install php7
RUN apt-get install -y php7.0 php7.0-xml
ADD resources/php/www.conf /etc/php/7.0/fpm/pool.d/www.conf
RUN /etc/init.d/php7.0-fpm start

#install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin/

#install postgresql
#ADD resources/postgresql/ACCC4CF8.asc /tmp/ACCC4CF8.asc
#RUN apt-key add ACCC4CF8.asc \
#	&& rm /tmp/ACCC4CF8.asc \
#	&& echo "deb http://apt.postgresql.org/pub/repos/apt/ xenial-pgdg main" >> /etc/apt/sources.list.d/postgresql.list \
#	&& apt-get update \
#	&& apt-get install -y postgresql-9.4

#install supervisor
RUN apt-get install -y supervisor
ADD resources/supervisor/conf.d/nginx.conf /etc/supervisor/conf.d/nginx.conf
ADD resources/supervisor/conf.d/php.conf /etc/supervisor/conf.d/php.conf

#create user system
RUN groupadd -f -g 1000 user \
	&& useradd -m -g user -u 1000 user

EXPOSE 80

CMD  ["/usr/bin/supervisord", "-n", "-c", "/etc/supervisor/supervisord.conf"]
FROM ubuntu:latest

ENV LANG en_US.utf8

RUN apt-get update && apt-get install -y locales \
    curl ufw\
    libzip-dev git unzip vim procps \
    nginx cron supervisor \
    php8.3 php8.3-cli php8.3-bz2 \
    php8.3-curl php8.3-mbstring php8.3-intl php8.3-fpm \
    && rm -rf /tmp/pear \
    && apt-get purge -y --auto-remove $buildDeps \
    && apt-get clean \
    && apt-get autoremove \
    && rm -rf /var/lib/apt/lists/* \
	&& localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8

COPY ./supervisor/supervisord.conf /etc/supervisord.conf
COPY ./nginx/default.conf /etc/nginx/conf.d/
COPY ./nginx/nginx.conf /etc/
RUN rm -r /etc/nginx/sites-available
RUN rm -r /etc/nginx/sites-enabled
COPY ./php/index.php /var/www/html/

COPY ./cron/crontab /etc/cron.d/
RUN /usr/bin/crontab /etc/cron.d/crontab

WORKDIR /root
RUN git clone https://github.com/seebi/dircolors-solarized

CMD supervisord -c /etc/supervisord.conf --nodaemon
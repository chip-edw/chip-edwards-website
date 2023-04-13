FROM ubuntu:latest
LABEL maintainer="chip@chip-edwards.com"

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=US/Chicago

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        apache2 \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /var/www/ChipWeb


COPY ./bootstrap ./bootstrap
COPY *.html ./
COPY *.css ./

RUN cp /etc/apache2/mods-available/rewrite.load /etc/apache2/mods-enabled/ && \
    cp /etc/apache2/mods-available/headers.load /etc/apache2/mods-enabled/ && \
    cp /etc/apache2/mods-available/ssl.load /etc/apache2/mods-enabled/ && \
    cp /etc/apache2/mods-available/ssl.conf /etc/apache2/mods-enabled/ && \
    cp /etc/apache2/mods-available/socache_shmcb.load /etc/apache2/mods-enabled/

COPY ./ApacheConfigs/ChipWeb/ChipWeb.conf /etc/apache2/sites-available/
COPY ./ApacheConfigs/ChipWeb/ChipWeb-le-ssl.conf /etc/apache2/sites-available/
COPY ./ApacheConfigs/ChipWeb/fullchain.pem /etc/letsencrypt/live/chip-edwards.com/
COPY ./ApacheConfigs/ChipWeb/privkey.pem /etc/letsencrypt/live/chip-edwards.com/
COPY ./ApacheConfigs/ChipWeb/options-ssl-apache.conf /etc/letsencrypt/


# Disable default site
RUN a2dissite 000-default
# Enable chip-edwards.com through ChipWeb.conf

RUN a2ensite ChipWeb
# Enable ssl after getting http:\\ to work
RUN a2ensite ChipWeb-le-ssl.conf

EXPOSE 80/tcp
EXPOSE 443/tcp

# Start Apache Server
CMD ["apachectl", "-D", "FOREGROUND"]
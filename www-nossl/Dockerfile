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
COPY *.ico ./

COPY ./ApacheConfigs/ChipWeb/ChipWeb.conf /etc/apache2/sites-available/



# Disable default site
RUN a2dissite 000-default
# Enable chip-edwards.com through ChipWeb.conf

RUN a2ensite ChipWeb


EXPOSE 80/tcp

# Start Apache Server
CMD ["apachectl", "-D", "FOREGROUND"]
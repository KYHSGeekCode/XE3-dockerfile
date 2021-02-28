FROM nginx:latest
LABEL Hyeonseoyang "yhs0602@snu.ac.kr"

ARG DEBIAN_FRONTEND=noninteractive

WORKDIR /var/www/html

RUN apt-get update
RUN apt-get install -yq --no-install-recommends apt-utils build-essential
RUN apt-get install -yq evince

ENV TZ=Asia/Seoul
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt install -yq  software-properties-common
RUN add-apt-repository ppa:ondrej/php
RUN apt-get update; exit 0
RUN apt-get install git wget unzip apache2 php7.2 php-cli php-fpm \
    php-mysql libapache2-mod-php php-curl php-gd php-json php-xml php-mbstring php-zip vim netcat -yq

RUN apt-get upgrade -yq

COPY .xe_install_config.yaml .xe_install_config.yaml
RUN php -r "copy('http://start.xpressengine.io/download/installer', 'installer');" && php installer install --config=.xe_install_config.yaml --no-interact
RUN chmod -R 707 storage/ bootstrap/ config/ vendor/ plugins/ index.php composer.phar

EXPOSE 80
STOPSIGNAL SIGTERM
# CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
CMD ["nginx", "-g", "daemon off;"]

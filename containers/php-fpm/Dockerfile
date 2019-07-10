FROM centos:7

RUN ln -sf /usr/share/zoneinfo/Japan /etc/localtime && \
    yum -y update && \
    yum -y install epel-release && \
    adduser -M -s /sbin/nologin -u 50000 wwwdata && \
    mkdir -p /var/www/html && \
    chown -R wwwdata:wwwdata /var/www/html && \
    /usr/bin/rm -rf /var/cache/yum/* && \
    yum clean all

RUN yum -y install https://rpms.remirepo.net/enterprise/remi-release-7.rpm && \
    /usr/bin/rm -rf /var/cache/yum/* && \
    yum clean all

RUN yum -y --enablerepo=remi-php73 install \
        php-bcmath \
        php-cli \
        php-common \
        php-fpm \
        php-gd \
        php-intl \
        php-json \
        php-mbstring \
        php-mysqlnd \
        php-pdo \
        php-pecl-swoole4 \
        php-xml \
        php-xmlrpc \
        php-opcache \
        php-pecl-apcu && \
    mkdir -p /run/php-fpm && \
    /usr/bin/rm -rf /var/cache/yum/* && \
    yum clean all

# ログ出力先を標準出力に変更
RUN ln -sf /dev/stderr /var/log/php-fpm/www-error.log && \
    ln -sf /dev/stderr /var/log/php-fpm/error.log

# 設定ファイルのデプロイ
COPY containers/php-fpm/config/php.ini /etc/php.ini
COPY containers/php-fpm/config/php-fpm.conf /etc/php-fpm.conf
COPY containers/php-fpm/config/www.conf /etc/php-fpm.d/www.conf

# アプリケーションのデプロイ
COPY ./sources/ /var/www/html

EXPOSE 9000
CMD ["/usr/sbin/php-fpm", "-c", "/etc/php-fpm.conf", "--nodaemonize"]

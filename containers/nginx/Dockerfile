FROM centos:7

RUN ln -sf /usr/share/zoneinfo/Japan /etc/localtime && \
    yum -y update && \
    yum -y install epel-release && \
    adduser -M -s /sbin/nologin -u 50000 wwwdata && \
    mkdir -p /var/www/html && \
    chown -R wwwdata:wwwdata /var/www/html && \
    /usr/bin/rm -rf /var/cache/yum/* && \
    yum clean all

COPY containers/nginx/include/nginx.repo /etc/yum.repos.d/nginx.repo

RUN yum -y install nginx && \
    /usr/bin/rm -rf /var/cache/yum/* && \
    yum clean all

# ログ出力先を標準出力に変更
RUN ln -sf /dev/stdout /var/log/nginx/access.log && \
    ln -sf /dev/stderr /var/log/nginx/error.log

# 設定ファイルのデプロイ
COPY containers/nginx/config/nginx.conf /etc/nginx/nginx.conf
COPY containers/nginx/config/default.conf /etc/nginx/conf.d/default.conf

# アプリケーションのデプロイ
COPY ./sources/ /var/www/html

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

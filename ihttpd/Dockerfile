FROM httpd:latest
RUN sed -i \
        -e 's/^#\(LoadModule proxy_module modules\/mod_proxy.so\)/\1/' \
        -e 's/^#\(LoadModule proxy_fcgi_module modules\/mod_proxy_fcgi.so\)/\1/' \
        -e 's/^#\(LoadModule rewrite_module modules\/mod_rewrite.so\)/\1/' \
        -e '$ a Include conf/vhost/*.conf' /usr/local/apache2/conf/httpd.conf \
    && mkdir -p /usr/local/apache2/conf/vhost/ \
    && mkdir -p /var/www/html/loeye-sample/htdocs
COPY image-files/ /
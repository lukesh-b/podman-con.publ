# Apache HTTP Server container using Red Hat UBI 10
FROM registry.redhat.io/ubi10:latest

# Apache will listen on this non-privileged port
ENV PORT=8080

# Install Apache HTTP Server
RUN dnf install -y httpd \
    && dnf clean all \
    && rm -rf /var/cache/dnf

# Copy static web content
COPY index.html /var/www/html/index.html

# Prepare runtime directories and set correct ownership
RUN mkdir -p /run/httpd /var/log/httpd \
    && chown -R apache:apache /run/httpd /var/log/httpd /var/www/html

# Configure Apache to listen on the specified port
RUN sed -ri "s/^[[:space:]]*Listen .*/Listen ${PORT}/I" /etc/httpd/conf/httpd.conf

# Suppress Apache FQDN warning
RUN echo "ServerName localhost.webserver" >> /etc/httpd/conf/httpd.conf

# Document the exposed port
EXPOSE 8080

# Run as non-root user
USER apache

# Start Apache in foreground
CMD ["/usr/sbin/httpd", "-DFOREGROUND"]


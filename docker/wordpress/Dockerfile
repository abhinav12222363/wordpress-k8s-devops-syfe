FROM wordpress:6.4-php8.2-apache

# Enable Apache rewrite (required by WordPress)
RUN a2enmod rewrite

# Set recommended PHP settings
RUN { \
    echo "upload_max_filesize=64M"; \
    echo "post_max_size=64M"; \
    echo "memory_limit=256M"; \
} > /usr/local/etc/php/conf.d/wordpress.ini

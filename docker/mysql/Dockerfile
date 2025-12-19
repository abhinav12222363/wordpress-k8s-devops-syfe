FROM mysql:8.0

# Set default MySQL charset & collation (production best practice)
RUN echo "[mysqld]\n\
character-set-server=utf8mb4\n\
collation-server=utf8mb4_unicode_ci" > /etc/mysql/conf.d/charset.cnf

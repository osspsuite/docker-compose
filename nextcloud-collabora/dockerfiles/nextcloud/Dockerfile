ARG nextcloud_version=apache
FROM nextcloud:${nextcloud_version}
RUN apt-get update && apt-get upgrade -y \
    && rm -rf /ar/lib/apt/lists/*
COPY entrypoint.sh /entrypoint.sh
COPY wait-for-it.sh /wait-for-it.sh
ENV NEXTCLOUD_UPDATE=1
ENTRYPOINT ["/entrypoint.sh"]
CMD ["apache2-foreground"]

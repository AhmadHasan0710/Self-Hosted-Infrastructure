version: "3.9"
services:
  nextcloud-db:
    image: mariadb:10.11
    container_name: nextcloud-db
    restart: unless-stopped
    environment:
      - MYSQL_ROOT_PASSWORD=<REDACTED>
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
      - MYSQL_PASSWORD=<REDACTED>
    volumes:
      - ./db:/var/lib/mysql

  nextcloud:
    image: nextcloud:latest
    container_name: nextcloud
    restart: unless-stopped
    depends_on:
      - nextcloud-db
    ports:
      - "127.0.0.1:8081:80"   # bound to localhost
    environment:
      - MYSQL_HOST=nextcloud-db
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
      - MYSQL_PASSWORD=<REDACTED>
      - TRUSTED_PROXIES=<CLOUDFLARE_TUNNEL_INTERNAL_RANGE>
    volumes:
      - ./data:/var/www/html

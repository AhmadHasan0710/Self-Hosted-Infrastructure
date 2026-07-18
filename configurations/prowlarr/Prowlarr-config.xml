version: "3.9"
services:
  prowlarr:
    image: linuxserver/prowlarr:latest
    container_name: prowlarr
    restart: unless-stopped
    environment:
      - PUID=<REDACTED>
      - PGID=<REDACTED>
      - TZ=<YOUR_TIMEZONE>
    volumes:
      - ./config:/config
    ports:
      - "127.0.0.1:9696:9696"   # bound to localhost (Worker Machine)


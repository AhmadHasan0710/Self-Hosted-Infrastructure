version: "3.9"
services:
  qbittorrent:
    image: linuxserver/qbittorrent:latest
    container_name: qbittorrent
    restart: unless-stopped
    environment:
      - PUID=<REDACTED>
      - PGID=<REDACTED>
      - TZ=<YOUR_TIMEZONE>
      - WEBUI_PORT=8080
    volumes:
      - ./config:/config
      - <DOWNLOADS_PATH>:/downloads
    ports:
      - "127.0.0.1:8080:8080"     # bound to localhost (Worker Machine)


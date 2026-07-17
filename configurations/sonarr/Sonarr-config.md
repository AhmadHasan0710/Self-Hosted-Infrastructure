version: "3.9"
services:
  sonarr:
    image: linuxserver/sonarr:latest
    container_name: sonarr
    restart: unless-stopped
    environment:
      - PUID=<REDACTED>
      - PGID=<REDACTED>
      - TZ=<YOUR_TIMEZONE>
    volumes:
      - ./config:/config
      - <TV_LIBRARY_PATH>:/tv
      - <DOWNLOADS_PATH>:/downloads
    ports:
      - "127.0.0.1:8989:8989"   # bound to localhost (Worker Machine)

W
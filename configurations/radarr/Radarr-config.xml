version: "3.9"
services:
  radarr:
    image: linuxserver/radarr:latest
    container_name: radarr
    restart: unless-stopped
    environment:
      - PUID=<REDACTED>
      - PGID=<REDACTED>
      - TZ=<YOUR_TIMEZONE>
    volumes:
      - ./config:/config
      - <MOVIES_LIBRARY_PATH>:/movies
      - <DOWNLOADS_PATH>:/downloads
    ports:
      - "127.0.0.1:7878:7878"   # bound to localhost (Worker Machine)


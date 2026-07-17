version: "3.9"
services:
  jellyseerr:
    image: fallenbagel/jellyseerr:latest
    container_name: jellyseerr
    restart: unless-stopped
    ports:
      - "127.0.0.1:5055:5055"   # bound to localhost
    volumes:
      - ./config:/app/config
    environment:
      - LOG_LEVEL=info
      - TZ=<YOUR_TIMEZONE>


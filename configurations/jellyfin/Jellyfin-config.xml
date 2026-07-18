version: "3.9"
services:
  jellyfin:
    image: jellyfin/jellyfin:latest
    container_name: jellyfin
    restart: unless-stopped
    ports:
      - "127.0.0.1:8096:8096"   # bound to localhost
    volumes:
      - ./config:/config
      - ./cache:/cache
      - <MEDIA_LIBRARY_PATH>:/media:ro
    # devices:
    #   - <HARDWARE_TRANSCODE_DEVICE>:<HARDWARE_TRANSCODE_DEVICE>

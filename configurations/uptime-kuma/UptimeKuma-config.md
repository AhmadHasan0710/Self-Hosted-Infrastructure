version: "3.9"
services:
  uptime-kuma:
    image: louislam/uptime-kuma:latest
    container_name: uptime-kuma
    restart: unless-stopped
    ports:
      - "127.0.0.1:3001:3001"   # bound to localhost
    volumes:
      - ./data:/app/data

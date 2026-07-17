version: "3.9"
services:
  website:
    image: <YOUR_WEBSITE_IMAGE>
    container_name: website
    restart: unless-stopped
    ports:
      - "127.0.0.1:8080:80"   # bound to localhost

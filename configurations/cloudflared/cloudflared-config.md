tunnel: <TUNNEL_ID>
credentials-file: <PATH_TO_CREDENTIALS_JSON>
ingress:
  ## Personal Site
  - hostname: example.com
    service: http://localhost:8080
    
  ## Jellyfin 
  - hostname: jellyfin.example.com
    service: http://localhost:8096

  ## Jellyseerr 
  - hostname: requests.example.com
    service: http://localhost:5055

  ## Nextcloud 
  - hostname: nextcloud.example.com
    service: http://localhost:8081

  ## Uptime Kuma 
  - hostname: kuma.example.com
    service: http://localhost:3001

  ## Catch-all rule required at the end of every ingress config
  - service: http_status:404

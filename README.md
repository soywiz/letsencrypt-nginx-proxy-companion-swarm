# letsencrypt-nginx-proxy-companion-swarm
Nginx reverse proxy with Letsencrypt, deployable to Docker swarm

### Setup:
1. Create a network called `webproxy`, using the `overlay` driver, and the `attachable` option
    - `docker network create webproxy --driver overlay --attachable`
2. Deploy the stack
    - `docker stack deploy -c nginx-letsencrypt-swarm.yml nginx-proxy`

### Example usage:
Deploy any subsequent services using port 80, attached to the `webproxy` network:
```
version: "3"
services:
  test-app:
    image: nginxdemos/hello
    deploy:
      replicas: 1
    environment:
      - VIRTUAL_HOST=nginxtest.example.com
      - LETSENCRYPT_HOST=nginxtest.example.com
    networks:
      - webproxy

networks:
  webproxy:
    external:
      name: webproxy
```

###CREATE DOCKERE-COMPOSE####
```
version: !!str 3
services:
  nginx:
    image: nginx:latest
    restart: unless-stopped

docker-compose up -d 
```
###CREATE DOCKER-COMPOSE FOR AUTHZ AND MYSQL####
```
version: !!str 3
services:
  authz:
    image: authz:1
    depends_on:
      - mysql
    env_file: .authz_env
    environment:
      - "SKOB_AUTHZ_USER_DEFAULT_STATUS=active"
    expose:
      - "8080"
    labels:
      - "local.dwsclass.sevice=authz"
    restart: unless-stopped
    networks:
      - nginx
      - database
  mysql:
    image: mysql:latest
    env_file: .mysql_env
    expose:
      - "3306"
    volumes:
      - mysql_data:/var/lib/mysql
    restart: unless-stopped
    networks:
      - database
volumes:
  mysql_data:
networks:
  database:
  nginx:
    external: true

```
### how to up 5 nginx with scale option
```
docker-compose up --scale nginx=5  -d 
```
### how  to  show serrvice logs
```
docker-compose logs -f  nginx
```

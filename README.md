
# Dockerfile
```
Dockerfile
```
```
FROM golang:alpine

RUN mkdir /app

WORKDIR /app

ADD go.mod .
ADD go.sum .

RUN go mod download
ADD . .

RUN go get github.com/githubnemo/CompileDaemon

EXPOSE ${PORT}

ENTRYPOINT CompileDaemon --build="go build main.go" --command=./main
```
```
docker build -t hello_world:1.0 .
```
```
docker run --rm -d -p 8000:8000 hello_world:1.0 --name go-blog
```
# .env

```
.env
```
```
MYSQL_ROOT_PASSWORD=root
DB_USER=user
DB_PASSWORD=*******
DB_HOST=db
DB_PORT=3306
DB_NAME=golang
SERVER_PORT=8000
ENVIRONMENT=local
PORT=8080
```

# docker-compose.yml mysql

```
docker-compose.yml
```
```yml 
version: '3'
services:
  db:
    image: mysql/mysql-server:5.7
    ports:
      - "3305:3306"
    restart: always
    environment:
      - "MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}"
      - "MYSQL_USER=${DB_USER}"
      - "MYSQL_PASSWORD=${DB_PASSWORD}"
      - "MYSQL_DATABASE=${DB_NAME}"   
  web:
    build: .
    ports:
      - "8000:${PORT}"
    volumes:
      - ".:/app"
    restart: always
    depends_on:
      - db
    links:
      - "db:database"
```

# docker-compose.yml postgres
```
docker-compose.yml
```

```yml 
version: '3.1'
services:
    db:
        image: postgres
        environment:
            POSTGRES_DB: ${DB_NAME}
            POSTGRES_USER: ${DB_USER}
            POSTGRES_PASSWORD: ${DB_PASSWORD}
        ports:
            - 6000:5432
        restart: always
    web:
        build: .
        ports:
          - "8080:${PORT}"
        volumes:
          - ".:/app"
        restart: always
        depends_on:
          - db
        links:
          - "db:database"
```

#mongo
```
version: '3.1'

services:

  mongo:
    image: mongo
    restart: always
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: password
    ports: 
        - 27017:27017
    volumes: 
        - .gomongodbdata:/data/db
```


#redis
```
version: '3.5'

services:
    redis:
        container_name: redis
        image: redis:6
        ports: 
            - 6379:6379
```

# .env 

```
.env
```
```
PORT=8080
DB_HOST=db
DB_USER=dnz-dev
DB_PASSWORD=dnz-dev
DB_NAME=dnz-dev
```

run command 
``` 
docker-compose up --build 
```

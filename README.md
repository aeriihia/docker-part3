# docker-part3

## 3.1

https://github.com/aeriihia/3.1

## 3.3

Dockerfile (Frontend)
```
FROM node:16
EXPOSE 5000
WORKDIR /usr/src/app
COPY . .
RUN npm install
RUN npm run build
RUN npm install -g serve
RUN useradd -m appuser
USER appuser
CMD ["serve", "-s", "-l", "5000", "build"]
```

Dockerfile (Backend)
```
FROM golang:1.16
EXPOSE 8080
WORKDIR /usr/src/app
COPY . .
RUN go build
RUN useradd -m appuser
USER appuser
CMD ./server
```

## 3.4

Frontend size before 1.2GB and after 516MB

Dockerfile (Frontend)
```
FROM ubuntu:18.04
EXPOSE 5000
WORKDIR /usr/src/app
COPY . .
RUN apt-get update && apt-get install -y curl && curl -sL https://deb.nodesource.com/setup_16.x | bash && apt install -y nodejs && npm install && npm run build && npm install -g serve && apt-get purge -y --auto-remove curl && rm -rf /var/lib/apt/lists/* && useradd -m appuser
USER appuser
CMD ["serve", "-s", "-l", "5000", "build"]
```

Backend size before 1.07GB and after 779MB

Dockerfile (Backend)
```
FROM ubuntu:18.04
EXPOSE 8080
WORKDIR /usr/src/app
COPY . .
RUN apt-get update && apt-get install -y curl && curl -OL https://golang.org/dl/go1.16.7.linux-amd64.tar.gz && tar -C /usr/local -xvf go1.16.7.linux-amd64.tar.gz && export PATH=$PATH:/usr/local/go/bin && go build && useradd -m appuser
USER appuser
CMD ./server
```

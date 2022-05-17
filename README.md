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

Backend size before 1.07GB and after 731MB

Dockerfile (Backend)
```
FROM ubuntu:18.04
EXPOSE 8080
WORKDIR /usr/src/app
COPY . .
RUN apt-get update && apt-get install -y curl && curl -OL https://golang.org/dl/go1.16.7.linux-amd64.tar.gz && tar -C /usr/local -xvf go1.16.7.linux-amd64.tar.gz && export PATH=$PATH:/usr/local/go/bin && go build && apt-get purge -y --auto-remove curl && rm -rf /var/lib/apt/lists/* && useradd -m appuser
USER appuser
CMD ./server
```

## 3.5

Frontend size before 1.2GB and after 402MB

Dockerfile (Frontend)
```
FROM node:16-alpine
EXPOSE 5000
WORKDIR /usr/src/app
COPY . .
RUN npm install && npm run build && npm install -g serve && addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser
CMD ["serve", "-s", "-l", "5000", "build"]
```

Backend size before 1.07GB and after 447MB

Dockerfile (Backend)
```
FROM golang:1.16-alpine
EXPOSE 8080
WORKDIR /usr/src/app
COPY . .
RUN go build && addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser
CMD ./server
```

## 3.6 part 1

Frontend size 122MB

Dockerfile (Frontend)
```
FROM node:16-alpine as build-stage
WORKDIR /usr/src/app
COPY . .
RUN npm install && npm run build && addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

FROM node:16-alpine
EXPOSE 5000
WORKDIR /usr/src/app
COPY --from=build-stage /usr/src/app/build /usr/src/app/build
RUN npm install -g serve
CMD ["serve", "-s", "-l", "5000", "build"]
```

## 3.6 part 2

Backend size 18MB

Dockerfile (Backend)
```
FROM golang:1.16-alpine AS build-stage
WORKDIR /usr/src/app
COPY . .
RUN CGO_ENABLED=0 go build && addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

FROM scratch
EXPOSE 8080
WORKDIR /usr/src/app
COPY --from=build-stage /usr/src/app/server /server
CMD ["/server"]
```

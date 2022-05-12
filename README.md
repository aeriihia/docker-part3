# docker-part3

## 3.1

https://github.com/aeriihia/3.1

## 3.3

Dockerfile (Frontend)
```
FROM node
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
# Docker for Development

## Introduction

Docker containerizes applications for consistent deployment. This tutorial covers Docker basics, Dockerfile, and containers.

---

## Dockerfile

    FROM node:16
    WORKDIR /app
    COPY package*.json ./
    RUN npm install
    COPY . .
    EXPOSE 3000
    CMD ["node", "index.js"]

---

## Commands

    docker build -t myapp .
    docker run -p 3000:3000 myapp
    docker ps
    docker stop container-id

---

## Conclusion

Use Docker for consistent development and deployment environments. Containerize applications for easy scaling.


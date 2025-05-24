# 🚀 Next.js + Docker + NGINX + SSL Production Setup Guide

This guide will help you containerize your Next.js frontend app with Docker, serve it using NGINX, secure it using Let's Encrypt SSL, and automate deployment via GitHub Actions.

---

## 📁 Folder Structure

```
nextjs-docker-prod/
├── app/                      # Next.js app folder
│   ├── Dockerfile
│   ├── package.json
│   └── ...
├── certbot/                  # For SSL certificates
│   ├── conf/
│   └── www/
├── nginx/                   # NGINX config
│   └── default.conf
├── docker-compose.yml
├── README.md
└── .gitignore
```

---

## 🐳 Docker Setup for Next.js (Production Ready)

### Dockerfile (`app/Dockerfile`)

```Dockerfile
# Stage 1: Build
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Stage 2: Run
FROM node:18-alpine AS runner
WORKDIR /app
COPY --from=builder /app/public ./public
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package.json ./package.json
EXPOSE 3000
CMD ["npm", "start"]
```

---

## 🧾 `.dockerignore`

```
node_modules
npm-debug.log
.next
.env.local
```

---

## 📦 `docker-compose.yml`

```yaml
version: "3"
services:
  frontend:
    build:
      context: ./app
    container_name: nextjs-app
    restart: always

  nginx:
    image: nginx:latest
    container_name: nginx
    restart: always
    volumes:
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
      - ./certbot/www:/var/www/certbot
      - ./certbot/conf:/etc/letsencrypt
    ports:
      - "80:80"
      - "443:443"
    depends_on:
      - frontend

  certbot:
    image: certbot/certbot
    volumes:
      - ./certbot/www:/var/www/certbot
      - ./certbot/conf:/etc/letsencrypt
```

---

## 🔧 `nginx/default.conf`

```nginx
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;
    location /.well-known/acme-challenge/ {
        root /var/www/certbot;
    }
    location / {
        proxy_pass http://frontend:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

After SSL setup, update it to:

```nginx
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name yourdomain.com www.yourdomain.com;

    ssl_certificate /etc/letsencrypt/live/yourdomain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;

    location / {
        proxy_pass http://frontend:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

---

## 📄 Get SSL Certificate

```bash
docker-compose up -d
docker-compose run certbot certonly \
  --webroot --webroot-path=/var/www/certbot \
  --email your-email@example.com \
  --agree-tos --no-eff-email \
  -d yourdomain.com -d www.yourdomain.com
```

---

## 🔄 SSL Auto-Renewal (Cron Job)

```bash
crontab -e
```

Add:
```
0 3 * * * docker-compose run certbot renew && docker-compose kill -s HUP nginx
```

---

## 🔁 GitHub CI/CD (GitHub Actions)

### `.github/workflows/deploy.yml`

```yaml
name: Deploy to VPS
on:
  push:
    branches:
      - main
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3
      - name: Deploy via SSH
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.VPS_HOST }}
          username: ${{ secrets.VPS_USER }}
          key: ${{ secrets.VPS_SSH_KEY }}
          script: |
            cd /home/youruser/nextjs-docker-prod
            git pull origin main
            docker-compose down
            docker-compose up -d --build
```

Add GitHub Secrets:
- `VPS_HOST`
- `VPS_USER`
- `VPS_SSH_KEY`

---

## 🧠 Daily Useful Docker Commands

| Task | Command |
|------|---------|
| Build image | `docker build -t app .` |
| Run container | `docker run -p 3000:3000 app` |
| List containers | `docker ps` |
| Shell into container | `docker exec -it container_id sh` |
| Stop container | `docker stop container_id` |
| Remove container | `docker rm container_id` |
| Clean up | `docker system prune` |

---

## 📝 Summary

- ✅ Multi-stage Dockerfile for Next.js
- ✅ NGINX reverse proxy + SSL
- ✅ Certbot auto-renewal
- ✅ GitHub CI/CD for automatic deployment

---

**Generated on: 2025-05-24**

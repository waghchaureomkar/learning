# 🚀 Complete Guide: Docker Setup for Next.js Frontend with NGINX, SSL & GitHub CI/CD

This README provides a full step-by-step guide to dockerizing a Next.js frontend application, adding NGINX as a reverse proxy, enabling HTTPS with Let's Encrypt, and setting up GitHub Actions for CI/CD deployment.

---

## 📁 Project Structure

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

## 🐳 Dockerfile for Next.js (Production)

Create `app/Dockerfile`:

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

## 📦 docker-compose.yml

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

## 🔧 NGINX Configuration

Create `nginx/default.conf`:

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

After getting SSL certificates, update this config to redirect HTTP to HTTPS and serve SSL:

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

## ✅ Get SSL Certificate (Let's Encrypt)

```bash
docker-compose up -d
docker-compose run certbot certonly \
  --webroot --webroot-path=/var/www/certbot \
  --email your-email@example.com \
  --agree-tos --no-eff-email \
  -d yourdomain.com -d www.yourdomain.com
```

---

## 🔁 SSL Auto-Renewal (Cron Job)

```bash
crontab -e
```

Add this to run every day at 3 AM:

```
0 3 * * * docker-compose run certbot renew && docker-compose kill -s HUP nginx
```

---

## ⚙️ GitHub CI/CD Pipeline

Create `.github/workflows/deploy.yml`:

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

---

### 🔐 GitHub Secrets Required

Go to your GitHub repo > **Settings → Secrets → Actions** and add:

- `VPS_HOST`: Your server IP
- `VPS_USER`: Your VPS username (e.g., root or ubuntu)
- `VPS_SSH_KEY`: Your SSH private key (not password)

---

## 🧾 .gitignore File

```gitignore
node_modules
.next
certbot/conf/*
certbot/www/*
.env*
```

---

## 🧠 Docker Commands for Daily Use

| Task | Command |
|------|---------|
| Build image | `docker build -t app .` |
| Run container | `docker run -p 3000:3000 app` |
| List containers | `docker ps` |
| View logs | `docker logs container_id` |
| Stop container | `docker stop container_id` |
| Remove container | `docker rm container_id` |
| Shell into container | `docker exec -it container_id sh` |
| Prune unused containers/images | `docker system prune` |

---

## 📝 Summary

- ✅ Multi-stage Dockerfile for Next.js
- ✅ Docker Compose with NGINX and Certbot
- ✅ Let's Encrypt SSL with auto-renewal
- ✅ GitHub Actions for CI/CD
- ✅ All production-ready

---

**Generated on: 2025-05-24**

# 🌐 Attach Domain to Local Port using NGINX + HTTPS

This guide helps you attach any custom domain (e.g. `app.example.com`) to a local port (e.g. `localhost:5000`) using **NGINX** as a reverse proxy and **Let's Encrypt (Certbot)** for free HTTPS.

---

## ✅ Prerequisites

- A domain or subdomain is already **pointed to your server's public IP** (via A or CNAME record)
- Your app is **running on a local port** (e.g. `localhost:5000`)
- You have **sudo access** to the server
- NGINX is installed

---

## 🧰 Step 1: Install Required Packages

```bash
sudo apt update
sudo apt install nginx certbot python3-certbot-nginx -y
```

---

## 📁 Step 2: Create NGINX Site Config

Replace `yourdomain.com` with your domain  
Replace `PORT_NUMBER` with the local port your app runs on

```bash
sudo nano /etc/nginx/sites-available/yourdomain.com
```

Paste the following config:

```nginx
server {
    listen 80;
    server_name yourdomain.com;

    location / {
        proxy_pass http://127.0.0.1:PORT_NUMBER;
        proxy_http_version 1.1;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

---

## 🔗 Step 3: Enable the Site

```bash
sudo ln -s /etc/nginx/sites-available/yourdomain.com /etc/nginx/sites-enabled/
```

---

## 🧪 Step 4: Test and Reload NGINX

```bash
sudo nginx -t
sudo systemctl reload nginx
```

---

## 🔐 Step 5: Enable HTTPS with Certbot

Run Certbot with your domain name:

```bash
sudo certbot --nginx -d yourdomain.com
```

Certbot will:
- Obtain an SSL certificate from Let’s Encrypt
- Automatically update your NGINX config to use HTTPS
- Redirect HTTP traffic to HTTPS

---

## ✅ Step 6: Access Your App

Visit your domain in a browser:

```
https://yourdomain.com
```

It will securely proxy to your app running on the local port you defined.

---

## 🔄 Step 7 (Optional): Test SSL Auto-Renewal

```bash
sudo certbot renew --dry-run
```

Certbot installs a cron job to handle automatic SSL renewal every 60–90 days.

---

## ♻️ Reuse Tip

To repeat this for another app or domain:

- Create a new NGINX file with a different domain and port
- Run Certbot again for that domain

---

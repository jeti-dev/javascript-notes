---
title: Linux
layout: default
---

## Ubuntu webserver
- ssh sammy@your_server_ip
    - ssh -l root -p22 123.12.123.12
- adduser sammy
    - usermod -aG sudo sammy
- ufw app list
    - ufw allow OpenSSH
    - ufw enable
    - ufw status

- sudo ufw allow 'Nginx HTTP'
- systemctl status nginx
- sudo systemctl stop nginx
- sudo systemctl start nginx
- sudo systemctl restart nginx
- if only config changed: sudo systemctl reload nginx
- not to start on server start: sudo systemctl disable nginx
 - sudo systemctl enable nginx
- sudo mkdir -p /var/www/your_domain/html
- sudo chown -R $USER:$USER /var/www/your_domain/html
- sudo chmod -R 755 /var/www/your_domain
- sudo nano /var/www/your_domain/html/index.html
- sudo nano /etc/nginx/sites-available/your_domain
- server {
        listen 80;
        listen [::]:80;

        root /var/www/your_domain/html;
        index index.html index.htm index.nginx-debian.html;

        server_name your_domain www.your_domain;

        location / {
                try_files $uri $uri/ =404;
        }
}
- sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/
- sudo nano /etc/nginx/nginx.conf
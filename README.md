# How-to-open-a-localhost-webserver-using-nginx.
How to open a localhost webserver using nginx.

# Install nginx web server.
```apt install nginx```


# Go to the desired directory and create the desired file.
``` nano /etc/nginx/sites-enabled/<file_name> ```


# Copy paste this line of code.
```
server {
    listen 443 ssl http2;  # Listen on HTTPS port 443 with HTTP/2
    server_name <ip>;  # Replace with your subdomain

    ssl_certificate /etc/certs/fullchain.pem;  # Path to Cloudflare Tunnel certificate
    ssl_certificate_key /etc/certs/privkey.pem;  # Path to Cloudflare Tunnel key

    # Optional security headers (consider adding these)
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload";
    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";

    location / {
        proxy_pass http://localhost:8080;  # Use port 8080 for Wings
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto https;
        proxy_buffering off;  # Improve performance for Wings
    }
}
```


# Restart nginx
```
systemctl restart nginx
```

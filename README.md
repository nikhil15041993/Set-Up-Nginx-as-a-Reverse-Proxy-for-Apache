# Set-Up-Nginx-as-a-Reverse-Proxy-for-Apache

## 1. Change Apache Listen Port

The first step is to change the port that Apache listens on. By default, most HTTP websites listen on port 80. If your website uses HTTPS, the default port is 443.

Make this change in the following places.

* /etc/apache2/ports.conf
* /etc/apache2/sites-available/000-default.conf

Apply your changes by reloading the Apache web server.
```
systemctl reload apache2
```

## 2. Install and Configure Nginx Reverse Proxy Server

If you don’t already have Nginx, you can install it with the apt package manager on Ubuntu and Debian.
```
apt install nginx
```

Once Nginx is installed, add the following proxy-related options to your Nginx configuration file. I’m using the default configuration files located at ``` /etc/nginx/sites-available/default```


```
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    root /var/www/html;
    # Add index.php to the list if you are using PHP
    index index.html index.htm index.nginx-debian.html;
    server_name _;
    location / {
        try_files $uri $uri/ =404;
        proxy_pass http://YOUR-IP-OR-DOMAIN:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

```
Notice how the proxy_pass line references port 8080. This is how the Nginx reverse proxy server will communicate with the Apache server.

Apply your changes by reloading Nginx.
```
systemctl reload nginx
```

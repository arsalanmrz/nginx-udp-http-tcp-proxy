### nginx-udp-http-tcp-proxy

#### Commands (for instance : ubuntu os)
- first update your os's packages : `sudo apt update`
- install nginx on your server : `sudo apt install nginx`
- enable nginx service : `sudo systemctl enable nginx`
- start nginx service : `sudo systemctl start nginx`
- try this command : `sudo nano /etc/nginx/nginx.conf` (like nginx.conf that I put it down for you)
- for save above file and exit the nano editor : press---> `ctrl+o` , `ctrl+m` , `ctrl+x`
- after all you should restart the nginx : `sudo systemctl restart nginx`
- At the end, you should set the dns of url : `proxy.mirbozorgi.com` to your ip's server.
- If you wanna have https (https://proxy.mirbozorgi.com):
    - `sudo apt update`
    - `sudo snap install core; sudo snap refresh core`
    - `sudo snap install --classic certbot`
    - `sudo ln -s /snap/bin/certbot /usr/bin/certbot`
    - `sudo certbot --nginx` (it will aks you some question , answer them with your personal data.)
    - `sudo certbot renew --dry-run` (cerbot is free and  your https will expire within 3 months,  with this,you run the automate cron for renew the SSL/TLS)

-  `https://proxy.mirbozorgi.com` is now available.

-  #### description:

    -  `proxy.mirbozorgi.com:3501` is redirect to `facebook.com:5222` with `udp` protocol.
    -  `proxy.mirbozorgi.com:3502` is redirect to `facebook.com:5223` with `udp` protocol.
    -  `proxy.mirbozorgi.com:3503` is redirect to `facebook.com:5224` with `tcp` protocol.
    -  `proxy.mirbozorgi.com:3504` is redirect to `facebook.com:5225` with `tcp` protocol.
    -  `https://proxy.mirbozorgi.com:8081` & `http://proxy.mirbozorgi.com:8081` are redirect to 
        `https://facebook.com/` with `http/https` protocols.


#### Sample for nginx.conf
``` js

user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
        worker_connections 1024;
}

stream {

    server {
        listen 3501 udp;
        proxy_pass facebook.com:5222;
    }


  server {
        listen 3502 udp;
        proxy_pass facebook.com:5223;
    }


  server {
        listen 3503;
        proxy_pass facebook.com:5224;
    }

  server {
        listen 3504;
        proxy_pass facebook.com:5225;
    }



}

http {
    server {
        listen       8081;
        location / {
            proxy_pass       https://facebook.com/;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header Host $host;
        }
    }
}


```
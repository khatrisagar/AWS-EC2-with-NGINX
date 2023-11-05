# AWS-EC2-with-NGINX


1. Create and Lanch EC2 instance and SSH into machine
```
create a EC2 instance based on requirement
create a elastic IP which is permenant IP and associate with EC2
```
2. Install Node and NPM
```
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install nodejs

node --version
```

3. Clone your project from Github
```
git clone ${url}
```
4. install dependencies and start app
```
sudo npm i pm2 -g
pm2 start index # here pm is process manager

# Other pm2 commands
pm2 show app
pm2 status
pm2 restart app
pm2 stop app
pm2 logs (Show log stream)
pm2 flush (Clear logs)

# To make sure app starts when reboot
pm2 startup ubuntu
```

5. setup firewall
```
sudo ufw enable
sudo ufw status
sudo ufw allow ssh (Port 22)
sudo ufw allow http (Port 80)
sudo ufw allow https (Port 443)
```
6. Install NGINX and configure
```
sudo apt install nginx
sudo nano /etc/nginx/sites-available/default
```
Add the following to the location part of the server block

```
 server_name yourdomain.com www.yourdomain.com;

    location / {
        proxy_pass http://localhost:8001; #whatever port your app runs on
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
```
```
# Check NGINX config
sudo nginx -t

# Restart NGINX
sudo nginx -s reload
```

7. Add SSL with certbot
```
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install python3-certbot-nginx
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com

# Only valid for 90 days, test the renewal process with
certbot renew --dry-run
```

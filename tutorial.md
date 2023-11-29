# Assignment3

## Starting from a Fresh Debian 12 server on digitalocean

Connect to server:<br>

```bash
ssh -i .ssh/do-key root@137.184.238.77
```

## Create a new regular user:

Add user:

```bash
useradd -ms /bin/bash ingyukim
```

Additional: Set password for the user:

```bash
passwd ingyukim
```


## User can perform administrative tasks

Append user into sudo group:

```bash
usermod -aG sudo ingyukim
```

## User has bash as login shell

copy /.ssh directory to home/ingyukim directory:

```bash
cp -r /root/.ssh /home/ingyukim/.ssh
```

Change ownership of the directory, and files in the directory so that the copy in your new users directory is owned by the new user and the new users primary group:

```bash
chown -R ingyukim:ingyukim /home/ingyukim/.ssh
```

## User can access the server via SSH

Connect to server with new user:

```bash
ssh -i .ssh/do-key ingyukim@137.184.238.77
```

## Prevent the root user from connecting to the server via SSH

Edit ssh configuration so that the root user can no longer connect to the server via ssh:

```bash
sudo vim /etc/ssh/sshd_config
```

```vim
PermitRootLogin no
```

Restart sshd to update my modification:

```bash
sudo systemctl restart sshd
```

## Install nginx

Install nginx:

```bash
sudo apt install nginx
```

## Configure nginx to serve a sample website

Documents to serve:

```bash
sudo mkdir my-site
```

```bash
sudo touch index.html
```

```bash
cd /etc/nginx/sites-available/
```

copy, paste and modify configuration file:

```bash
sudo vim my-site.conf
```

```vim
server {
	listen 80 default_server;
	listen [::]:80 default_server;

	root /var/www/my-site;
	
	index index.html index.htm index.nginx-debian.html;
	
	server_name _;
	
	location / {
		# First attempt to serve request as file, then
		# as directory, then fall back to displaying a 404.
		try_files $uri $uri/ =404;
	}
}
```

Sysbolic link:

```bash
sudo ln -s /etc/nginx/sites-available/my-site.conf /etc/nginx/sites-enabled
```

```bash
cd /etc/nginx/sites-enabled
```

Unlink default:

```bash
sudo unlink default
```

```bash
sudo nginx -t
```

restart nginx to up to date:

```
sudo systemctl restart nginx
```

Test:

```
curl 137.184.238.7
```

# 2420 Assignment 2

## Step 1: Creating the Digital Ocean Infrastructure 

1. Create a VPC Network  and rename it `vpc-2420`

![VPC](/images/vpc-2420.png)

Note: Remember which server your VPC Network is on, as it will be the same server location for the droplets and load balancer  
  
2. Create 2 new droplets called `ubuntu-1` and `ubuntu-2` with the new VPC Network with a tag called `web`

![droplets](/images/droplets.png)

Note: the tag will be used to apply the load balancer and firewall to the droplets with the same tag

3. Create a load balancer on the same VPC Network and select the tag `web`

![load balancer](/images/loadbalancer.png)

4. Create a firewall via the Cloud on Digital Ocean and select the tag `web`

![firewall](/images/fw-2420.png)

## Step 2: Creating Regular Users 

Create new regular users for each droplet

1. Run the command `ssh-keygen -t ed25519 -C <email>`  
where <email> = your email address
2. Enter into your server via `ssh -i .\.ssh\<key> root@<ip>` where the `key` is the name of your key and `ip` is the droplet's ip address
3. Run the command `useradd -ms /bin/bash <name>`, where name is the `name` of your regular user
4. Run the command `usermod -aG sudo <name>`
5. Run the command `rsync --archive --chown=<name>:<name> ~/.ssh /home/<name>`
6. Run the command passwd <name> to set your password for the regular user
7. Run `exit`

8. ssh -i .\.ssh\<key> <name>@<ip>
9. sudo vim /etc/ssh/sshd_config  
`SET PermitRootLogin no`
10. Run the command `sudo systemctl restart ssh`
11. Run `exit`

## Step 3: Installing Web Server

In the droplets, we will install a web server. We will use caddy.

1. Download the tar.gz file for caddy by running `wget https://github.com/caddyserver/caddy/releases/download/v2.6.2/caddy_2.6.2_linux_amd64.tar.gz`
2. Unarchive the file with `tar xvf caddy_2.6.2_linux_amd64.tar.gz`
3. Run `chown <user>: caddy`
4. Run `sudo cp caddy /usr/bin`
5. Create a 'caddy' directory in /etc/ with `sudo mkdir /etc/caddy`
6. Create a Caddyfile in /etc/caddy with `sudo vim /etc/caddy/Caddyfile`
7. Create a service file to run `sudo caddy run --config /etc/caddy/Caddyfile`

![caddy file image](/images/caddyfile.service.png)

## Step 4

In your local machine:

1. Make a directory called `2420-assign-two`
2. Inside this directory, create `html` and `src`
3. Inside `html`, create an index html that is simple and complete (with doctype, head, body)
4. Inside of src, create an index.js file like below:

[!index.js file]('./images/index.js.png')
5. Test your server locally by trying to view your index.html with index.js
6. Move html and src directory to both of the Digital Ocean servers.  
sfpt the files by running `sfpt -i <ssh_key_path> <user>@<ip>`  
then run the command `put -r <directory>`

## Step 5

1. Write your Caddyfile on your local machine  
`http://<ip_of_load> {  
    root * /var/www
    reverse_proxy /app localhost:3000  
    file_server
}  
`

## Step 6

## Step 7

## Step 8

## Step 9

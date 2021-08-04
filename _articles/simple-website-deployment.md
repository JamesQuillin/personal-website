---
layout: default
title: Simple Website Deployment
description: A step-by-step introduction to deploying your website where you control the full stack.
tags: VPS SSH Git Nginx Beginner-Friendly Language-Agnostic
---
# A Simple Way to Deploy a Website

([TL;DR;](/articles/simple-website-deployment#tldr) avalable).

This approach was really simple for me to figure out when I first started, and it's an approach I still like to use for my own projects since it's cheap, quick, and easy. 

You basically rent an internet-connected Linux computer for $5/mo, install and run your app, and it's online. It's especially great for beginners, who might have just finished a tutorial and want to try deploying their project. 

Seeing your first website live on the internet is extremely satisfying and once you can get something online you can go in almost any direction you want from there. This approach also makes it super easy to add a real domain name if you want (they cost ~$15/year for the good ones) as well as SSL encryption (HTTPS) so that will be in here as well.

Whether it's a side project or even your first tutorial, whether it's React, or Rails, or NodeJS or anything else, if you can get it to run on localhost then you can get it onto the internet with this approach so if that sounds interesting then keep reading.

## Overview

Here's a quick overview of what we are doing:

- rent a cheap virtual server
- connect with SSH
- install your app with git
- run your app and see it online
- **\*optional\*** hook it up with a domain name and HTTPS (~$15/year for the domain name)

At the end I'll include a list of all the commands in one chunk as a quick reference.

## Setting up a "Droplet" with DigitalOcean

First we set up the virtual machine, which DigitalOcean calls a "droplet". This provides us with the hardware that we need. You could definitely do this part yourself and try to host your site from home but that is a completely different beast and a large project in and of itself.

- Create a droplet and choose Ubuntu for the image (the most user-friendly Linux distribution and really easy to google questions for)

- Next, select the lowest specs possible to end up with the cheapest option (it charges like $0.007 an hour and averages to $5/mo if you keep the site up, but you can always delete it whenever so it's really flexible and inexpensive)

- Skip "Block Storage"

- Choose a data center near you

- Skip over "VPC Network" and "Additional Options"

- Add an SSH key. If you don't know what an SSH key is or how to set one up, check out [this post](/articles/ssh), or for a quick reminder:

- ```
ssh-keygen -t rsa
cat ~/.ssh/id_rsa.pub
# copy-paste the output into the form
```

- Finally, name it whatever you want, skip the rest of the options, and create the droplet

## Connecting with SSH

Now you can copy the IP address of the droplet and log in with `ssh root@server_ip`. It'll be a bunch of numbers so like `ssh root@108.177.8.119` (which just redirects to google so make sure to use yours!). 

If you are using a Mac or Linux then you can just use the built in terminal for SSH. For Windows I would use the terminal that comes with Git (which you will want anyway if you don't already have it).

## Installing Depenencies

Now we install whatever our app needs to run. If it's a Ruby on Rails app then install Ruby. If it's NodeJS + Express then Node, etc. 

If you aren't sure how to install something, just google "ubuntu install ___" and look for instructions that use `apt` or `apt-get` (the built in package manager for Ubuntu) to install the software.

Here is my example app:

```js
// index.js - a NodeJS and Express App

const express = require('express');
const app = express();

app.get('/', (req, res) => res.send('Hello World!'));

app.listen(3000, () => console.log('listening on port 3000.'));
```

To run mine I'll need NodeJS and NPM:

```
apt update
apt install nodejs
apt install npm
```

I'll also use NPM to install PM2. If you just run your app with something like `node app.js` then node will just crash when the session for your terminal ends. 

PM2 manages this for you by running node (or whatever runs your app) in the background. It can also restart the process automatically, for instance if your droplet has to restart, so it's really convenient.

If your app isn't a NodeJS app, you can still install NPM to get PM2, or you can probably install PM2 with whatever other package manager your app uses.

```
npm install -g pm2
```

## Cloning your Code

Now we can download our app. This section assumes you have a repository set up on Github (it can be public or private). If you don't or you don't know how to set it up, check out [this post](/articles/quick-github-intro) for how to do that.

Start by creating an SSH key on your droplet the same way you did on your regular computer. You can use `cat` to print out the public key so you can copy-paste it to Github. Then you can easily clone the code from Github straight to your droplet. 

Once you have your code, move into the folder, install your dependencies, and run your code using PM2.

```
ssh-keygen -t rsa
cat ~/.ssh/id_rsa.pub

git clone git@github.com:UserName/repo-name.git

cd repo-name
npm install # don't forget to install your app
pm2 start node index.js
```

It is now live on the internet! Just go to `server_ip:port_number` (e.g. `108.177.8.119:3000`). You could technically stop here but I'd encourage you to go on, especially if a domain name you like is avavilable. 

If you do stop here consider running your app on port 80 so that users don't need to type the `:3000` part, since port 80 is the default port for regular HTTP requests.

### Sidenote:

If you can't get `pm2` to play nicely (I had trouble getting it to work with Jekyll once) you can always run the app in the background using the `&` operator, e.g.:

```
bundle exec jekyll serve &
```

To stop it you can run `ps aux` to find the process id and then `kill [pid]` to stop it. This lets you keep your app running once you close your shell (like with pm2), though it's much less robust and you might need to restart it yourself or with a script, like if the server crashes or restarts.

## Adding a Domain Name

This part is optional since your app is now online, but can really make it feel much more like a real website, since you get a name instead of a bunch of numbers, and you can easily encrypt your connection for free (HTTPS) once you have a domain name.

First things first. 

For this part we need to purchase a domain name. They usually cost about $15 a year for a good one (like yourname.com) but there are also some cheaper alternatives (like .me for around $5 a year or .xyz for $1 a year). 

There are a couple of places you can buy them from but my favorite is NameCheap. I have bought multiple domain names from them and it's always a quick and easy experience with a clean UI.

Once you have the domain name, you need to point the domain name at DigitalOcean's nameservers. This will let you add the domain name to the droplet. Check out [this guide](https://www.digitalocean.com/community/tutorials/how-to-point-to-digitalocean-nameservers-from-common-domain-registrars) for how to do that.

(If you also used NameCheap, just go to your domain names, then `Manage` and select `Custom DNS` and type in):

```
ns1.digitalocean.com
ns2.digitalocean.com
ns3.digitalocean.com
```

From there you have to wait for everything to catch up and update and then you can move on with adding it to your droplet and adding HTTPS to your site. The guide I linked says it can take 30 minutes to a couple of hours to update. The first time I did it it took about 20 minutes.

Once that's finished you can add the domain name to your droplet and now you can visit `yourwebsitename.whatever:3000`, instead of `108.177.8.119:3000`! Remember to use port `:80` so you can leave off the port number if you stop here. 

If you made it to this point though, I *highly* recommend that you keep going for the last section. It shows you how to add HTTPS to your site at no cost, and it's really simple to do.

## Adding HTTPS with Certbot and Nginx

Certbot is a program that will generate an SSL certificate for you for free and set it up automatically. You might be able to set it up alone without Nginx but it doesn't hurt to have Nginx in the middle between your app and the internet anyway, and doing it this way is really simple and lets them handle the configuration for you, which is great as a beginner.

As for Nginx, we are setting it up as a proxy. All requests come in through Nginx which then can easily reroute them to your app on whatever port you have it running. 

(Of course you could also just use Nginx to serve a site as a folder of static files. If you wanted to do that you would change the `location` directive to route the requests appropriately).

```
apt install certbot
apt install python3-certbot-nginx

cd /etc/nginx/conf.d    # move to the configuration directory
touch sitename.com.conf # make a configuration file for your site
nano sitename.com.conf  # open the file in nano to edit it 
                        # (or use vi/vim/emacs if you like pain)
```

Either type or paste in the content below. Make sure to replace `sitename.com` and `:3000` with your domain name and the port your app is going to use. Use `ctrl+o` and press `enter` to save, and `ctrl+x` to exit nano.

```
server {
  listen 80;
  listen [::]:80;

  server_name sitename.com;

  location / {
    	proxy_pass http://localhost:3000/;
  }
}
```

Finally we run the following commands to test and update the basic Nginx configuration and to then run Certbot, which generates our SSL certificate and updates the Nginx config to use it for us.

When you run the certbot command it will ask you a few questions, but the one that matters to you is when it asks if you want to redirect all traffic to HTTPS, and make sure you type in the menu option to have it redirect all HTTP requests to HTTPS for you.

```
nginx -t && nginx -s reload
sudo certbot --nginx -d sitename.com
```

## All Done!

With that you're all done and can now visit your app, online, with HTTPS, and with a real domain name!

## TL;DR; 

### Part 1: Deploying Your App
```
# --- set up SSH and connect to your server
ssh-keygen -t rsa
cat ~/.ssh/id_rsa.pub
ssh root@server_ip

# --- install your app's software
apt update
apt install nodejs
apt install npm

# --- install PM2 to run your app
npm install -g pm2

# --- get your server an SSH key for Github
ssh-keygen -t rsa
cat ~/.ssh/id_rsa.pub

# --- clone and install your app
git clone git@github.com:UserName/repo-name.git
cd repo-name
npm install

# --- run your app with PM2
pm2 start node index.js
```

### Part 2: Add a Domain Name and HTTPS (optional)

```
# --- point your domain name to DigitalOcean's nameservers
ns1.digitalocean.com
ns2.digitalocean.com
ns3.digitalocean.com

# --- grab some lunch while you wait for the DNS to get updated

# --- set up nginx and certbot for HTTPS
apt install certbot
apt install python3-certbot-nginx

cd /etc/nginx/conf.d
touch sitename.com.conf
nano sitename.com.conf

    server {
      listen 80;
      listen [::]:80;

      server_name sitename.com;

      location / {
        proxy_pass http://localhost:3000/;
      }
    }

nginx -t && nginx -s reload
sudo certbot --nginx -d sitename.com
```

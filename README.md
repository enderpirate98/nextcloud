## Sections
- Introduction
- Installing and configuring Nginx Proxy Manager(npm)
- Initial Nextcloud AIO install and OnlyOffice AIO addon
- Nextcloud configuration
- Using Nextcloud on Client devices
## Section 0: Introduction

Have you ever heard of the [unfortunate soul whose digital life got destroyed by Google](https://www.nytimes.com/2022/08/21/technology/google-surveillance-toddler-photo.html) over a misunderstood medical photo? With Nextcloud, you can take control of your data and escape the reach of Big Tech by hosting your own cloud drive.

Depending on how much storage you server has you might be able to backup your whole life for free!

## Section 1: Installing Nginx Proxy Manager(npm)

First lets create the data directory for npm
```
sudo mkdir -p /srv/dockerdata/NGINXProxyManager/data && sudo mkdir /srv/dockerdata/NGINXProxyManager/letsencrypt
```
Open up Portainer from CasaOS and create a secure admin account, click on ``Get Started`` and then ``local``

Install the docker container graphically using [this video](https://www.youtube.com/watch?v=fCJbw75DCZw&t=467s) at 1:08 in portainer

After the install go to http://192.168.your.ip:81 , the default user name is ``admin@example.com`` and the default password is ``changeme``

After you login a popup will prompt you to change the username and password, please do so

You can use your own domain that you purchased but in this guide we will use a free duckdns subdomain along with setting it up with dynamic dns because most of the time resedential internet does not come with a public static ip

Go to [duckdns.org](https://duckdns.org), create an account, and claim your own subdomain, also save your token for later

Now go to ``Install`` > ``linux cron`` and follow the instructions for pointing your Debian server to duckdns

Now that we have Dynamic DNS setup with DuckDNS we need to do some port forwarding

Search up something like ``how to port forward brand modelno`` and forward the ports 80 and 443 from your Debian server

Back in npm go to ``Hosts`` and click ``Add Proxy Host``, set the scheme to ``https``, for now the ip will ``172.0.0.1`` but it will be changed when the appropriate container is created, the port to forward is ``11000``, enable ``Block Common Exploits`` and ``Websockets Support``, the domain name will be ``nc.chosenname.duckdns.org``

Go to the ``ssl`` tab and ``Request a new SSL Certificate``, enable ``Force SSL``, ``HTTP/2 Support``, and ``Use a DNS Challenge``. 

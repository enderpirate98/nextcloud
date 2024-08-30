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

After the install go to http://192.168.your.ip:81

more to come

sooooooon...

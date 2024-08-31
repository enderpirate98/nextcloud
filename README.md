## Sections
- Introduction
- Installing and configuring Nginx Proxy Manager(npm)
- Initial Nextcloud AIO install and OnlyOffice AIO addon
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

Go to the ``ssl`` tab and ``Request a new SSL Certificate``, enable ``Force SSL``, ``HTTP/2 Support``, and ``Use a DNS Challenge``. For the DNS Provider choose ``DuckDNS`` and put the token that you got earlier, provide your personal email and click ``I Agree to the Let's Encrypt terms and service`` and we have the networking portion done.

## Section 2: Installing Nextlcoud AIO and OnlyOffice AIO

First we need to create the directories that will store Nextcloud AIO with the data, configs, and backups
```
sudo mkdir -p /srv/dockerdata/nextcloudAIO/config && sudo mkdir /srv/dockerdata/nextcloudAIO/data && sudo mkdir /srv/dockerdata/nextcloudAIO/backups
```
This command will install the Nextcloud AIO Mastercontainer

```
docker run \
--sig-proxy=false \
--name nextcloud-aio-mastercontainer \
--restart always \
--publish 82:80 \
--publish 8090:8080 \
--publish 8443:8443 \
--env APACHE_PORT=11000 \
--volume nextcloud_aio_mastercontainer:/srv/dockerdata/nextcloudAIO/config \
--volume /var/run/docker.sock:/var/run/docker.sock:ro \
--volume /srv/dockerdata/nextcloudAIO/data:/mnt/docker-aio-config \
--volume /srv/dockerdata/nextcloudAIO/backups:/mnt/backup
nextcloud/all-in-one:latest
```

Go to ``https://192.168.your.ip:8090`` and it will greet you with your mastercontainer password, write it down and keep it somewhere safe, under ``New AIO Instance`` put ``nc.chosenname.duckdns.org`` and submit that domain.

Uncheck ``Collabora (Nextcloud Office)`` since we will use OnlyOffice, select anything else that you want and hit ``Save changes`` and input your timezone.

Finally check the box to install the latest stable version of Nextcloud and hit ``Download and start containers``. After all of the containers have downloaded and loaded, go and shut them all down, in the CasaOS file manger go to ``/srv/dockerdata/nextcloudAIO/config`` and edit the ``configuration.json``, find ``isOnlyofficeEnabled`` and change the 0 to a 1, in portainer go to the ``containers`` page and reboot the master container. 

In the ``Backup and Restore`` section put ``/mnt/backup`` and click ``Submit Backup location``.

After that you can boot all of the containers back up, in npm click on the 3 dots button next to your ``nc.chosenname.duckdns.org`` and edit the ip to be the ip of the ``nextcloud-aio-apache`` container, the ip can be found in the portainer ``containers`` page.

Click ``save`` and you have a funtioning Nextcloud instance that you can access from ``https://nc.chosenname.duckdns.org``!!!

## Section 3: Using Nextcloud on Client Devices

I will finish this later but for now this is pretty good!

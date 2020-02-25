---
title: Setting SSL/TSL
updated: 2020-01-28 00:00
---

## Select certificate

I was using many providers for long time, but in the last times I discover [certbot](https://certbot.eff.org). Cerbot genera the SSL/TLS certifacte automatic and **free** for diferent services. 

### Install SSL/TLS certifacte for Apache2 in Ubuntu 18.04.


```
$ sudo apt install certbot
$ sudo apt-get install python-certbot-apache
$ sudo certbot --apache -d <mydomain.com> [-d <mydomain2.com>]*
```


### Install SSL/TLS certifacte for Nginx in Ubuntu 18.04.

```
$ sudo apt install certbot
$ apt-get install python-certbot-nginx
```

You must setting the domain, probably

```
$ sudo nano sites-enabled/default
server {
  charset utf-8;
  server_name <mydomain> <mydomain2>;
.....

$ nginx -t && nginx -s reload
$ sudo certbot --nginx -d <mydomain> -d <mydomain>

```
* Remaind if you hava a Firewall like ufw, you must allow the port 443. 


### Automatically Renew

Let's Encrypt certificates expire after 90 days, but it has easy solution, create a cron task.
```
$ crontab -e
0 12 * * * /usr/bin/certbot renew --quiet
```

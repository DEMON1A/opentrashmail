<p align="center">
  <a href="" rel="noopener">
 <img height=200px src="https://raw.githubusercontent.com/HaschekSolutions/opentrashmail/master/web/imgs/logo_300.png" alt="Open Trashmail"></a>
</p>

<h1 align="center">Open Trashmail</h1>

<div align="center">

![](https://img.shields.io/badge/php-7.1%2B-brightgreen.svg)
![](https://img.shields.io/badge/python-2.7%2B-brightgreen.svg)
[![](https://img.shields.io/docker/pulls/hascheksolutions/opentrashmail?color=brightgreen)](https://hub.docker.com/r/hascheksolutions/opentrashmail)
[![](https://img.shields.io/docker/cloud/build/hascheksolutions/opentrashmail?color=brightgreen)](https://hub.docker.com/r/hascheksolutions/opentrashmail/builds)
[![Apache License](https://img.shields.io/badge/license-Apache-blue.svg?style=flat)](https://github.com/HaschekSolutions/opentrashmail/blob/master/LICENSE)
[![HitCount](http://hits.dwyl.io/HaschekSolutions/opentrashmail.svg)](http://hits.dwyl.io/HaschekSolutions/opentrashmail)
[![](https://img.shields.io/github/stars/HaschekSolutions/opentrashmail.svg?label=Stars&style=social)](https://github.com/HaschekSolutions/opentrashmail)

#### Host your own `trashmail` solution to use with your own domains or subdomains

</div>

![Screenshot of Open Trashmail](https://pictshare.net/shz4tq.png)

# Features
- Python powered mail server that works out of the box for any domain you throw at it
- API for integrating it in your own projects. Can be used to give Users individual email adresses and read what they send to it
- Handles attachments
- Web interface to manage emails
- Generates random email adresses
- 100% file based, no database needed
- Can be used as Email Honeypot

# Configuration
Just edit the `config.ini` You can use the following settings

- `DOMAINS` -> Comma separated list of domains this mailserver will be receiving emails on. It's just so the web interface can generate random addresses
- `MAILPORT`-> The port the Python powered SMTP server will listen on. Default 25
- `ADMIN` -> An email address (doesn't have to exist, just has to be valid) that will list all emails of all addresses the server has received. Kind of a catch-all
- `DATEFORMAT` -> How should timestamps be shown on the web interface ([moment.js syntax](https://momentjs.com/docs/#/displaying/))

# Roadmap
- [x] Mailserver
  - [x] Storing received mails in JSON
  - [x] Storing file attachments
- [x] Docker files and configs
- [ ] Web interface
  - [x] Choose email
  - [x] Get random email address
  - [x] Download attachments in a safe way
  - [x] Display Text/HTML
  - [x] API so all features from the site can also be automated and integrated
  - [x] Automatically check for new emails while on site
  - [x] Admin overview for all available email addresses
  - [ ] Secure HTML so no malicious things can be loaded
  - [ ] Display embedded images inline using Content-ID
  - [ ] Option to show raw Email

  - [ ] Delete messages
  - [ ] Make better theme
- [ ] Configurable settings
  - [x] Choose domains for random generation
  - [ ] Choose if out-of-scope emails are discarded
  - [ ] Honeypot mode where all emails are also saved for a catchall account
  - [ ] Optionally secure whole site with a password
  - [ ] Optinally allow site to be seen only from specific IP Range

# Quick start

Simple start with no persistence

```bash
docker run -it -p 25:25 -p 80:80 hascheksolutions/opentrashmail
```

Saving data directory on host machine

```bash
docker run -p 80:80 -p 25:25 -v /path/on/host/where/to/save/data:/var/www/opentrashmail/data hascheksolutions/opentrashmail
```

Complete example with running as daemon, persistence, a domain for auto-generation of emails and auto restart

```bash
docker run -d --restart=always --name opentrashmail -e "DOMAINS=mydomain.eu" -e "DATEFORMAT='D.M.YYYY HH:mm'" -p 80:80 -p 25:25 -v /path/on/host/where/to/save/data:/var/www/opentrashmail/data hascheksolutions/opentrashmail
```

# How it works

The heart of Open Trashmail is a **python powered SMTP server** that listens on incoming emails and stores them as json files. The server doesn't have to know the right Email domain, it will just **catch everything** it receives. You only have to **expose port 25 to the web** and set an **MX record** of your domain pointing to the IP adress of your machine.

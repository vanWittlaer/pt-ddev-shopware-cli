# **DDEV and shopware-cli**

<img src="images/ddev-logo.svg" alt="DDEV Logo" class="ddev-logo">
<img src="images/shopware-logo-brand-blue.svg" alt="Shopware Logo">

## Why not get most out of both?

Note:
Speaker notes go here.

---

## Agenda

1. What is DDEV in short
2. DDEV shopware-cli integration
3. DDEV add-ons, commands and hooks
4. A minimalistic Shopware setup
5. A full-blown Shopware setup

Note:
Speaker notes go here.

---

## What is DDEV in short

- Docker-based OS-agnostic development platform for web projects
- 20k+ weekly users (based on telemetry data)

<img src="images/usage-bars.png" alt="DDEV usage chart" height="300px" />

<a href="https://ddev.com/usage-stats/#" target="_blank">`https://ddev.com/usage-stats/#`</a>

--

## Architecture

<img src="images/architecture-overview.png" alt="DDEV architecture" height="400px" />

<a href="https://ddev.com/blog/ddev-docker-architecture/" target="_blank">https://ddev.com/blog/ddev-docker-architecture/</a>

--

## Getting Started

* Install DDEV
* Create and move to new project folder
* Run `ddev config`
* Run `ddev start`

--

## Project Structure

* The `.ddev` folder holds everything related to DDEV's project configuration
* Is the basis to provide a 1:1 development environment to every team member
* Brings its own `.gitignore` - all non-committed files can be recovered with `ddev start`
* Basic configuration is in `config.yaml`.
* Can be extended with any custom configuration or additional files.

--

## Simple config.yaml

```yaml
name: open-stage
type: php
docroot: ""
php_version: "8.4"
webserver_type: nginx-fpm
xdebug_enabled: false
additional_hostnames: []
additional_fqdns: []
database:
    type: mariadb
    version: "11.8"
use_dns_when_possible: true
composer_version: "2"
web_environment: []
nodejs_version: "24"
# Change to 'true' to gain access to latest versions of yarn/pnpm
corepack_enable: false
```

--

## The fritz.box Trap

* fritz.box routers have a feature 'DNS Rebinding Protection'
* This prevents resolution of *.ddev.site domains
* Fix: Add ddev.site to fritz.box DNS settings
* Or point your host's DNS to 1.1.1.1 or 8.8.8.8 or similar
* In case DDEV detects this it automatically adds the necessary DNS entries to `/etc/hosts` - which, however, requires admin rights
* See <a href="https://ddev.com/blog/fritzbox-routers-and-ddev/" target="_blank">https://ddev.com/blog/fritzbox-routers-and-ddev/</a>

---

## DDEV shopware-cli integration


Use the `shopware-6` project type to tell DDEV to set up a Shopware 6 project environment

* Creates a `.env.local` within your composer root folder
* Bakes shopware-cli latest version into the web container
* Many commands work flawlessly and out of the box, like the watchers or the validate command
* But then not all commands work or are useful in DDEV (like `project dev`)

--

## .env.local

```yaml
DATABASE_URL="mysql://db:db@db:3306/db"
APP_ENV="dev"
MAILER_DSN="smtp://127.0.0.1:1025?encryption=&auth_mode="
APP_URL="https://<your-project-name>.ddev.site"
```

--

## Getting started

See
* <a href="https://docs.ddev.com/en/stable/users/quickstart/#shopware" target="_blank">https://docs.ddev.com/en/stable/users/quickstart/#shopware</a> or
* <a href="https://notebook.vanwittlaer.de/ddev-for-shopware/less-than-5-minutes-install-with-ddev-and-symfony-flex" target="_blank">https://notebook.vanwittlaer.de/ddev-for-shopware/less-than-5-minutes-install-with-ddev-and-symfony-flex</a>

Use shopware-cli via `ddev shopware-cli` (or `ddev ssh` into the web container and use `shopware-cli` directly)

--

## Storefront and Admin Watchers

`ddev storefront-watch` (port 9998)

`ddev admin-watch <plugin-directory>` (relative to working_dir, port 5173)

--

## Media Proxy

Can easily be achieved with a nginx config (provided `webserver_type: nginx-fpm`)

```nginx
# .ddev/nginx/media-proxy.conf

    set $media_proxy_url "https://media.example.com/public";
    
    location @mediaserver {
        resolver 1.1.1.1 ipv6=off;
        proxy_ssl_server_name on;
        proxy_pass $media_proxy_url$request_uri;
    }
    
    location ^~ /media/ {
        access_log off;
        expires max;
        try_files $uri $uri/ @mediaserver;
        break;
    }
    
    location ^~ /thumbnail/ {
        access_log off;
        expires max;
        try_files $uri $uri/ @mediaserver;
        break;
    }
```
Similar setups for Apache or other webservers are possible.

--

## Good to know

Any script running inside the DDEV web container can use the IS_DDEV_PROJECT environment variable to detect DDEV projects.

---

## How to tailor DDEV to your needs

* Add-ons
* Commands
* Hooks
* Providers

--

### DDEV Add-ons

<a href="https://docs.ddev.com/en/stable/users/extend/using-add-ons/" target="_blank">https://docs.ddev.com/en/stable/users/extend/using-add-ons/</a>

* Add-ons are used e.g. to add ElasticSearch or Redis to a project (see example below)
* Easy to create your own add-on for whatever you need

--

### DDEV Custom Commands

<a href="https://docs.ddev.com/en/stable/users/extend/custom-commands/" target="_blank">https://docs.ddev.com/en/stable/users/extend/custom-commands/</a>

* Used to make your life easier
* Can run on host, in web or db container

--

### DDEV Hooks

<a href="https://docs.ddev.com/en/stable/users/configuration/hooks/" target="_blank">https://docs.ddev.com/en/stable/users/configuration/hooks/</a>

* The post-import-db hook can be used to script e.g. sales channel domain changes
* Note: core config settings can also be fixed using Shopware system config yaml

--

### DDEV Providers

<a href="https://docs.ddev.com/en/stable/users/providers/" target="_blank">https://docs.ddev.com/en/stable/users/providers/</a>

* Providers can be used with `ddev pull` and `ddev push` commands to pull/push data between your local and remote environments
* Anything that goes with ssh is feasible on the remote

---

## A minimalistic Shopware setup

<a href="https://github.com/vanWittlaer/sissy-demo" target="_blank">https://github.com/vanWittlaer/sissy-demo</a>

* Just the Shopware production template, nginx, PHP 8.4, MariaDB 11.8

---

## A 'full-blown' Shopware setup

<a href="https://github.com/vanWittlaer/swoofy" target="_blank">https://github.com/vanWittlaer/swoofy</a>

* ... plus: Redis, Elasticsearch and RabbitMQ

---

# Thank You

Questions?

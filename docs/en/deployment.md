# Deployment

> :bell: Strongly recommended that Deploy Zealot using a [Docker](https://www.docker.io/),
> unless you are familiar with the technology stack for this service.
> Given that iOS uses a download service that relies on opening SSL/TLS certificates,
> it is recommended to use an authorized certificate service, such as [Let's Encrypt](https://letsencrypt.org/),
> and if you use a self-signed certificate you must install the self-signed certificate
> on each iOS device before downloading and installing the application.
## Why support docker image only?

Deploying a Rails-based application is incredibly complex, and even though you can have Ruby,
ImageMagick, Node, Postgres, and Redis installed on your deployment server,
you still need to worry about running the Zealot service and the Sidekiq asynchronous task service.

The Docker image provided by the project takes all this hassle and puts it into the image to do the initialization via a one-click deployment and installation script.

## Hardware requirements

- One CPU at least, More is better
- 512 MB+ memory
- 64 bit Linux OS
- 20 GB+ disk space

## Software requirements

- Docker 20.10.0+
- Docker Compose 1.28.0+

## Install on Docker

In the principle of one-click installation, but reality is often harsh,
Zealot configuration is dependent on ENV environment variables,
you need to configure it and then execute the one-click deployment generation script.

First you need to clone the [deployment script](https://github.com/tryzealot/zealot-docker.git),
After entering the `zealot-docker` directory, you need to open the `example.env` file to
configure the necessary parameters and then you can directly execute `. /deploy.sh` script.

> By default, the administrator account: `admin@zealot.com` and password `ze@l0t`
> and some demo applications will be generated.

```bash
$ git clone https://github.com/tryzealot/zealot-docker.git
$ cd zealot-docker
$ ./deploy
```

The one-click deployment generation script has three built-in templates by default:

- Using Let's Encrypt SSL
- Using self-signed SSL
- Using Reverse proxy with SSL

For those interested in one-click installation deployment scripts,
you can check out the [Deployment Documentation with Docker](docker.md).

### Using Let's Encrypt SSL (Best Recommand)

**Step 1**: Execute the deployment script:

```bash
$ ./deploy
```

**Step 2**: Check and configure the `.env` file, mainly whether `ZEALOT_DOMAIN` and `ZEALOT_CERT_EMAIL` are filled in correctly.
Other parts can be adjusted according to the actual situation of the corresponding configuration

**Step 3**: Run the Zealot service:

```bash
$ docker-compose up -d
```

### Self-signed SSL

Please do not use this for non-essential cases, for iOS using self-signed certificates
**may require the device to also have an SSL certificate installed before accessing and installing the application**,
and Chrome may also deny access due to the certificate.

> If the domain name is unregistered, you need to tie the host to access it,
> usually by modifying the system's `/etc/hosts` file.

```bash
$ sudo vim /etc/hosts

127.0.0.1 zealot.test
```

### Using reverse proxy

If you already have a gateway or load balance such as [Nginx](http://nginx.org/),
[Caddy](https://caddyserver.com/) for SSL certificate proxy, you can run it.
After running, get the IP address of the `zealot-zealot` instance on port 80 and reverse proxy it to the above service.

#### Nginx config file

> The following is the general configuration, if not available welcome to [file a issue](https://github.com/tryzealot/zealot-docs/issues/new)。

```
server {
  listen 443;
  server_name localhost;

  ssl                 on;
  ssl_certificate     /etc/certs/zealot-cert.key;
  ssl_certificate_key /etc/certs/zealot.key;
  ssl_session_timeout 5m;
  ssl_session_cache   shared:SSL:1m;

  root /app/public;

  location ~* ^(/assets|/favicon.ico) {
    access_log        off;
    expires           max;
  }

  location / {
    proxy_redirect     off;
    proxy_set_header   Host               $host:$server_port;
    proxy_set_header   X-Forwarded-Host   $host:$server_port;
    proxy_set_header   X-Forwarded-Port   $server_port;
    proxy_set_header   X-Forwarded-Server $host;
    proxy_set_header   X-Real-IP          $remote_addr;
    proxy_set_header   X-Forwarded-For    $proxy_add_x_forwarded_for;
    proxy_buffering    on;
    proxy_pass         http://127.0.0.1;
  }

  location /cable {
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_pass http://127.0.0.1;
  }

  error_page 500 502 503 504 /500.html;
  location = /500.html {
    root /app/public;
  }
}
```

#### Caddy 2 config file

```
:443

log

## Using Let's Encrypt
tls zealot@exampl.com

reverse_proxy 127.0.0.1:80
```

The configuration only needs to relate the ip part after `tls` and `proxy`.

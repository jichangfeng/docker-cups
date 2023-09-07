# CUPS Docker Image

## Overview

Docker image including CUPS print server and printing drivers (installed from the Debian packages).

## Architectures

- linux/amd64
- linux/386
- linux/arm64/v8
- linux/arm/v7

## Run the Cups server

```bash
# Run the container
docker run -d --restart always -p 631:631 -v $(pwd):/etc/cups jichangfeng/cups:latest
```

```bash
# Run the container with the specified architecture
docker run --platform linux/386 -d --restart always -p 631:631 -v $(pwd):/etc/cups jichangfeng/cups:latest
```

The `/etc/cups` directory is the directory used to store configuration files and related settings for CUPS.
Choose to mount `/etc/cups` to the container so that the added printers will not be lost after the container is destroyed and restarted.

## CUPS web interface

Login in to CUPS web interface on [https://127.0.0.1:631](https://127.0.0.1:631) and configure CUPS to your needs.
Default credentials: admin / admin

To change the admin password set the environment variable `ADMIN_PASSWORD` to your password.

```bash
docker run -d --restart always -p 631:631 -v $(pwd):/etc/cups -e ADMIN_PASSWORD=AnySecretPassword jichangfeng/cups:latest
```

## Configure Cups client on your machine

1. Install the `cups-client` package
2. Edit the `/etc/cups/client.conf`, set `ServerName` to `127.0.0.1:631`
3. Test the connectivity with the Cups server using `lpstat -r`
4. Test that printers are detected using `lpstat -v`
5. Applications on your machine should now detect the printers!

For more usage, refer to [Printer Sharing / Automatic Configuration using IPP](https://www.cups.org/doc/sharing.html#AUTO_IPP) 

## Included package

* cups, cups-client, cups-bsd, cups-filters
* foomatic-db
* printer-driver-all, printer-driver-cups-pdf
* openprinting-ppds
* hpijs-ppds, hp-ppd
* sudo, whois, inetutils-ping
* smbclient

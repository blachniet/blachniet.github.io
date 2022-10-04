---
title: "Failed to get D-Bus connection"
date: "2017-05-28"
categories:
- "site-updates"
tags:
- "google-cloud-platform"
- "linux"
aliases:
- "/blog/failed-to-get-d-bus-connection/"
blachnietWordPressExport: true
---

After setting up the [systemd service for Caddy](https://github.com/mholt/caddy/tree/master/dist/init/linux-systemd), I received the following error when trying to start the service:

```shell
$ sudo systemctl start caddy.service
Job for caddy.service failed. See 'systemctl status caddy.service' and 'journalctl -xn' for details.
$ systemctl status caddy.service
Failed to get D-Bus connection: No such file or directory
```

The fix was pretty easy. It turns out that dbus was not installed on the system. I installed it and then the service worked perfectly:

```shell
$ apt-get install -y dbus
```

For what it's worth, I did this on a [Google Cloud Compute Engine](https://cloud.google.com/compute/) instance that I initialized with the default Debian install.

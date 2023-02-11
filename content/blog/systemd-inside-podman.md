+++
title = "Run systemd inside Podman "
date = 2023-02-11
+++

If you use a Linux distrubiton that uses SELinux (eg. Fedora Linux) the first thing you have to do is to let it know that you are allowing to run systemd inside Podman.

You have to change the `container_manage_cgroup` bool.

To do this run `setsebool -P -N container_manage_cgroup 1`

Then you have to build your Containerfile which installs and initalize the init system.

Example:

```
FROM fedora:37

RUN dnf -y install systemd; dnf clean all

CMD [ "/sbin/init" ]
```

To build the image run `podman build -t systemd .`

And you can start your container now.

`podman run -d systemd:latest`

To enter the container run `podman exec -ti CONTAINER_NAME /bin/bash`

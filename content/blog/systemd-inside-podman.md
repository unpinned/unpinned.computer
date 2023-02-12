+++
title = "Run systemd inside Podman "
date = 2023-02-11
+++

For Linux distributions that use SELinux (eg. Fedora Linux) the first thing you have to do is to let SELinux know that you are allowing to run systemd inside a Podman container.

You have to change the `container_manage_cgroup` bool.

To do this run `setsebool -P -N container_manage_cgroup 1`

Then you have to build your Containerfile which installs and initalizes systemd.

Most minimal example:

```dockerfile
FROM fedora:37

RUN dnf -y install systemd; dnf clean all

CMD [ "/sbin/init" ]
```

To build the image run `podman build -t systemd .`

And you can start your container now.

`podman run --name my-systemd-container -d systemd:latest`

To enter the container run `podman exec -ti my-systemd-container /bin/bash`

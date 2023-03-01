+++
title = "Run systemd inside Podman "
date = 2023-02-11
+++

## UPDATE: Apparently you don't have to change bools anymore! Its supported out of the box now! [See this](https://github.com/coreos/fedora-coreos-tracker/issues/397#issuecomment-1343214681)

<strike>For Linux distributions that use SELinux (eg. Fedora Linux) the first thing you have to do is to let SELinux know that you are allowing to run systemd inside a Podman container.</strike>

<strike>You have to change the ` ̶c̶o̶n̶t̶a̶i̶n̶e̶r̶_̶m̶a̶n̶a̶g̶e̶_̶c̶g̶r̶o̶u̶p̶` bool.</strike>

<strike>To do this run ` ̶s̶e̶t̶s̶e̶b̶o̶o̶l̶ ̶-̶P̶ ̶-̶N̶ ̶c̶o̶n̶t̶a̶i̶n̶e̶r̶_̶m̶a̶n̶a̶g̶e̶_̶c̶g̶r̶o̶u̶p̶ ̶1̶`</strike>

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

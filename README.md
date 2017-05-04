# docker_ubuntu_mate

## What is docker_ubuntu_i3 anyway?

It is a Ubuntu zesty image with x11vnc, novnc and i3. You can use it as a desktop or as the basis for a more customized

## Example Usage

### Basic

```
docker run -d -P rigormortiz/ubuntu_i3:zesty
```

### Persistent home directory

```
docker volume create ubuntu_desktop_home
docker run --rm -v ubuntu_desktop_home:/desktop_tmp rigormortiz/ubuntu_i3:latest cp -pr /home/ubuntu /desktop_tmp
docker run -d -P -v ubuntu_desktop_home:/home rigormortiz/ubuntu_i3:zesty
```
### Static port

```
docker run -d -p 5900:5900 -p 6080:6080 rigormortiz/ubuntu_i3:zesty
```

## Noteworthy

### Capabilities
i3bar wants access to certain system calls/capabilities that are not available to docker containers by default. The default i3bar configuration will throw an error (which can safely be ignored) if you don't add the `CAP_SYS_ADMIN` capability via the `--cap-add` flag to the `docker run` command. Alternatively you can run the container in priveleged mode. I generally try to avoid that, but it is an option!

### Exposed ports

- x11vnc is exposed on port `5900`
- noVNC's http interface is exposed on port `6080`

### Misc.
- Default user is `ubuntu`
- Default VNC password is currently the same as `DESKTOP_USERNAME`. See TODO in next section. It can be easily changed at runtime with:

```
x11vnc -storepasswd somePassword /home/${DESKTOP_USERNAME}/.vnc/passwd
```

### NoVNC base image
This image is based on my ubuntu_novnc image. If you are using it to build your own image you may want to be familiar with it as some of the settings we use here are set in that image (like username) due to necessity/architecture.

### TODO

- support for ssl
- dynamically generated VNC password

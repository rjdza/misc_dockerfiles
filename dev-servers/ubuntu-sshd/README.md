# Ubuntu OpenSSH Server

> [!WARNING]
> Remove or modify the section that adds my ssh keys.

## BUILD:
docker build -t ubuntu-ssh-server .

## RUN:
docker run -d -p 2222:22 --name myubuntu --hostname teleport-ssh-test-0x ubuntu-ssh-server

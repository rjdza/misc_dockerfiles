# Ubuntu + RunIt + OpenSSH Server

> [!WARNING]
> Remove or modify the section that adds my ssh keys.

##BUILD:
docker build -t ubuntu-runit-sshd .

##RUN:
docker run -d -p 2222:22 --name runit-sshd --hostname runit-sshd ubuntu-runit-sshd

#Alpine OpenSSH Server

> [!WARNING]  
> Remove or modify the section that adds my ssh keys.

##BUILD:
docker build -t alpine-sshd .

##RUN:
docker run -d -p 2222:22 --name alpine-sshd-XX --hostname alpine-sshd-test-01 alpine-sshd

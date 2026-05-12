# Ubuntu + Teleport AIO

> [!WARNING]
> Remove or modify the section that adds my ssh keys.

## BUILD:
docker build -t ubuntu-teleport-aio .

## RUN:
docker run -d -p 3085:3080 --name teleport-aio --hostname teleport-aio ubuntu-teleport-aio

## Common Tasks
### Create a Teleport user and set up multi-factor authentication

1. Open a shell in your container:
```BASH
docker exec -it <CONTAINER_ID> /bin/bash
```

2. On your VM or container, run the following command (remove sudo if using a local container). tctl is a client tool for configuring the Teleport Auth Service:
```BASH
sudo tctl users add teleport-admin --roles=editor,access,auditor --logins=root,ubuntu,ec2-user
```

The command prints a message similar to the following:
```BASH
User "teleport-admin" has been created but requires a password. Share this URL with the user to complete user setup, link is valid for 1h:
https://teleport.example.com:443/web/invite/123abc456def789ghi123abc456def78

NOTE: Make sure teleport.example.com:443 points at a Teleport proxy which users can access.
```

Make sure the host and port match your setup.

3. Visit the provided URL in order to create your Teleport user.

The tctl command you ran earlier created a local Teleport user with the following preset roles:

| Role    | Permissions |
|---------|---------------------------------------------|
| editor  | Perform administrative tasks in your cluster|
| access  | Connect to any Teleport-protected resource  |
| auditor | View audit events and session recordings    |

Since this user has almost total privileges, you should treat this identity as a fallback after completing initial setup operations, and define other roles and identities for day-to-day usage.



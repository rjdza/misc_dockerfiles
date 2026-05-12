FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

# Install runit + openssh + utilities
RUN apt-get update && apt-get install -y --no-install-recommends \
    runit \
    openssh-server \
    ca-certificates \
    curl \
    vim \
    mc \
    && rm -rf /var/lib/apt/lists/*

# Prepare SSH runtime directory
RUN mkdir -p /var/run/sshd

# Create SSH directory for root
RUN mkdir -p /root/.ssh && chmod 700 /root/.ssh

# Fetch Deploy_Key_01
RUN curl -fsSL -L "https://raw.githubusercontent.com/rjdza/pubkeys/refs/heads/main/Deploy_Key_01" \
    -o /root/.ssh/authorized_keys

# Fetch Deploy_Key_02 (append)
RUN curl -fsSL -L "https://raw.githubusercontent.com/rjdza/pubkeys/refs/heads/main/Deploy_Key_02" \
    >> /root/.ssh/authorized_keys

# Secure permissions
RUN chmod 600 /root/.ssh/authorized_keys

# Configure SSH for key-only login
RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config && \
    sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config && \
    sed -i 's/#PubkeyAuthentication yes/PubkeyAuthentication yes/' /etc/ssh/sshd_config

# -----------------------------
# Add runit service for openssh
# -----------------------------
RUN mkdir -p /etc/service/openssh-server

# run script
RUN printf '#!/bin/sh\nexec /usr/sbin/sshd -D -e\n' \
    > /etc/service/openssh-server/run && \
    chmod +x /etc/service/openssh-server/run

# Expose SSH port
EXPOSE 22

# runit stage 2 init
CMD ["runsvdir", "-P", "/etc/service"]



FROM alpine:3.19

# Install packages
RUN apk update && apk add --no-cache \
    openssh \
    curl \
    vim \
    mc

# Create SSH directory
RUN mkdir -p /root/.ssh && chmod 700 /root/.ssh

# Fetch your SSH public key dynamically
RUN curl -fsSL "https://raw.githubusercontent.com/rjdza/pubkeys/refs/heads/main/Deploy_Key_01" \
    -o /root/.ssh/authorized_keys

RUN curl -fsSL "https://raw.githubusercontent.com/rjdza/pubkeys/refs/heads/main/Deploy_Key_02" >> /root/.ssh/authorized_keys

# Secure permissions
RUN chmod 600 /root/.ssh/authorized_keys

# Configure SSH for key-only login
RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config && \
    sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config && \
    sed -i 's/#PubkeyAuthentication yes/PubkeyAuthentication yes/' /etc/ssh/sshd_config

# Generate SSH host keys
RUN ssh-keygen -A

# Expose SSH port
EXPOSE 22

# Start SSH daemon
CMD ["/usr/sbin/sshd", "-D"]


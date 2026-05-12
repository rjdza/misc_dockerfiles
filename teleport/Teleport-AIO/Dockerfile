FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

# Install dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    ca-certificates \
    vim \
    mc \
    libnss3-tools \
    && rm -rf /var/lib/apt/lists/*

# Install mkcert
RUN curl -fsSL https://github.com/FiloSottile/mkcert/releases/latest/download/mkcert-v1.4.4-linux-amd64 \
    -o /usr/local/bin/mkcert && \
    chmod +x /usr/local/bin/mkcert

# Prepare Teleport TLS directory
RUN mkdir -p /etc/teleport-tls

# Generate local CA + localhost cert
RUN mkcert -install && \
    cd /etc/teleport-tls && \
    mkcert localhost && \
    cp "$(mkcert -CAROOT)/rootCA.pem" .

# Install Teleport
RUN curl https://cdn.teleport.dev/install.sh | bash -s 18.7.6

# Generate Teleport config
RUN teleport configure -o file \
    --cluster-name=homenet \
    --public-addr=tunnel01.homenet.za.org:443 \
    --cert-file=/etc/teleport-tls/localhost.pem \
    --key-file=/etc/teleport-tls/localhost-key.pem 

# Expose Teleport web/SSH port
EXPOSE 443

# Run Teleport as PID1
CMD ["teleport", "start", "--config=/etc/teleport.yaml"]

LABEL maintainer="RJDZA"
LABEL teleport.deploy.url="https://goteleport.com/docs/get-started/deploy-community/"
LABEL teleport.config.url="https://goteleport.com/docs/"
LABEL common.commands.config="teleport configure -o file \
	--cluster-name=[MY_CLUSTER_NAME] \
	--public-addr=[IP_OR_FQDN]:443 \
	--cert-file=/etc/teleport-tls/localhost.pem \
	--key-file=/etc/teleport-tls/localhost-key.pem"
LABEL common.commands.start="teleport start --config=/etc/teleport.yaml"
LABEL purpose="Teleport All-In-One"


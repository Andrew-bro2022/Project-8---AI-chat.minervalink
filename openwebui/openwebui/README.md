# OpenWebUI


## VPS side Ubuntu

Install packages before add host.

```bash
sudo apt install unzip
```

Install docker

Install Prerequisite Packages

```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

Add Docker’s Official GPG Key

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

Add the Docker Repository

```bash
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

Install Docker

```bash
sudo apt install docker-ce
```

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

## OpenWebUI Host

```bash
docker network create openwebui_network
```

```bash
docker compose up
```

```bash
docker exec -it ollama ollama pull nomic-embed-text
```

```bash
docker exec -it ollama ollama pull llama3
```

## Create SSL certificates for Nginx server, here is using Let's Encrypt

 Install Certbot and the Nginx Plugin

```bash
sudo apt update
sudo apt install certbot
```

Obtain SSL Certificates

```bash
sudo certbot certonly --standalone -d nexusforcare.com
```

```bash
cp /etc/letsencrypt/live/nexusforcare.com/fullchain.pem /home/hostuser/openwebui/nginx/certs/hostname-domain.crt
```

```bash
cp /etc/letsencrypt/live/nexusforcare.com/privkey.pem /home/hostuser/openwebui/nginx/certs/hostname-domain.key
```

After docker compose up
Set Up Automatic Certificate Renewal:Certbot's certificates are valid for 90 days, so you need to renew them regularly. Set up a cron job to handle this. Edit the crontab for root

```bash
sudo crontab -e
```

```bash
0 0 * * * certbot renew --post-hook "cp /etc/letsencrypt/live/nexusforcare.com/fullchain.pem /home/hostuser/openwebui/nginx/certs/hostname-domain.crt && cp /etc/letsencrypt/live/nexusforcare.com/privkey.pem /home/hostuser/openwebui/nginx/certs/hostname-domain.key && docker-compose restart nginx"
```

## Issues

In manage Ollama Mode, default is http://host.dcoker.internal:11434 or http://host.dcoker.internal:11434/api, this is for Ollama is in installed in host server. 
For Ollama in docker container, it is http://ollama:11434 while http://{ollama_docker_container_name}:11434
# Deployment Guide for Spring PetClinic Microservices

This guide explains how to deploy the application on an EC2 instance from scratch.

## 1. Prerequisites

Before you begin, ensure that the target host has:
- A recent Ubuntu server (22.04 or 24.04)
- Docker Engine and the Docker Compose plugin
- Git
- Optional: Docker Hub credentials if you want to publish images
- At least 4 GB of RAM and enough disk space for the stack

## 2. Connect to the EC2 instance

```bash
ssh -i your-key.pem ubuntu@YOUR_EC2_PUBLIC_IP
```

## 3. Install required packages

```bash
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg git unzip
```

Install Docker by following the official Docker documentation for Ubuntu, or use the Docker repository packages if you prefer.

## 4. Install Docker and Compose

After installing Docker, enable and start the service:

```bash
sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -aG docker $USER
newgrp docker
```

Verify the install:

```bash
docker --version
docker compose version
```

## 5. Clone the repository

```bash
cd ~
git clone https://github.com/hanumanbhalerao2077/spring-petclinic-devops.git
cd spring-petclinic-devops
```

## 6. Prepare environment variables

Copy the example environment file and update it:

```bash
cp .env.example .env
nano .env
```

Set the required values:
- `OPENAI_API_KEY` (optional for local demo use)
- `AZURE_OPENAI_KEY` (optional)
- `AZURE_OPENAI_ENDPOINT` (optional)

## 7. Build the application images

From the repository root:

```bash
docker compose build
```

If you want to build only a subset of services, you can target a specific service name.

## 8. Start the full stack

```bash
docker compose up -d
```

This starts:
- Config Server
- Discovery Server (Eureka)
- Customers Service
- Visits Service
- Vets Service
- GenAI Service
- API Gateway
- Admin Server
- Prometheus
- Grafana

## 9. Verify the deployment

Check container status:

```bash
docker compose ps
```

Open the following URLs in your browser:
- API Gateway: http://YOUR_EC2_PUBLIC_IP:8080
- Eureka: http://YOUR_EC2_PUBLIC_IP:8761
- Config Server: http://YOUR_EC2_PUBLIC_IP:8888
- Prometheus: http://YOUR_EC2_PUBLIC_IP:9091
- Grafana: http://YOUR_EC2_PUBLIC_IP:3030

## 10. CI/CD workflow

The repository includes a GitHub Actions workflow in `.github/workflows/maven-build.yml`.

It performs:
- Maven test and package steps
- Docker image build steps
- Optional Docker Hub push when the required secrets are configured
- A Trivy scan step that reports vulnerabilities without blocking the workflow by default

Required repository secrets for image push:
- `DOCKERHUB_USERNAME`
- `DOCKERHUB_TOKEN`

## 11. Troubleshooting common issues

### Docker daemon not running
```bash
sudo systemctl status docker
```

### Port already in use
```bash
sudo ss -ltnp | grep 8080
```

### Service failed to start
```bash
docker compose ps
docker compose logs <service-name>
```

### Missing environment variables
Make sure `.env` is present and contains the values expected by `docker-compose.yml`.

## 12. Final deployment checklist

- [ ] Docker is installed and running
- [ ] `.env` is populated correctly
- [ ] `docker compose build` completes successfully
- [ ] `docker compose up -d` starts all services
- [ ] Health checks and endpoints are reachable
- [ ] Prometheus and Grafana are reachable
- [ ] CI/CD workflow is configured in GitHub

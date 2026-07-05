# Deployment Checklist

Use this checklist before deploying the application to a fresh environment.

## 1. Prerequisites
- [ ] Ubuntu 22.04/24.04 or a compatible Linux host
- [ ] Docker Engine and Docker Compose plugin installed
- [ ] Git installed
- [ ] Java 17 (only if building locally without Docker)
- [ ] OpenSSH access to the host
- [ ] Optional: Docker Hub credentials for image publishing

## 2. Server preparation
- [ ] Create a non-root user with sudo access
- [ ] Update the OS and install required packages
- [ ] Open required ports: 8080, 8761, 8888, 9090, 9091, 3030, 9411
- [ ] Ensure the host has enough memory for the full stack

## 3. Repository and configuration
- [ ] Clone the repository
- [ ] Copy `.env.example` to `.env` and set the required variables
- [ ] Review `docker-compose.yml` and ensure the ports are available

## 4. Docker build and startup
- [ ] Build the required images from the repository root
- [ ] Start the stack with Docker Compose
- [ ] Confirm service health checks pass

## 5. Verification
- [ ] API Gateway: http://host:8080
- [ ] Eureka: http://host:8761
- [ ] Config Server: http://host:8888
- [ ] Prometheus: http://host:9091
- [ ] Grafana: http://host:3030

## 6. CI/CD readiness
- [ ] GitHub Actions workflow is present in `.github/workflows/maven-build.yml`
- [ ] Required repository secrets are configured if Docker Hub push is enabled
- [ ] Trivy scan step is present and non-blocking by default

## 7. Post-deployment
- [ ] Review logs for startup errors
- [ ] Confirm the application is reachable via the gateway
- [ ] Save the deployment commands for future reference

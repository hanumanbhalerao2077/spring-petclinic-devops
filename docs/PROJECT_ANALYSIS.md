# Project Analysis and Architecture Guide

## What this project is
Spring PetClinic Microservices is a sample distributed application built to demonstrate how a classic monolithic Spring application can be split into independent microservices. The project uses Spring Boot, Spring Cloud, Spring Cloud Gateway, Eureka, Config Server, Docker, Prometheus, Grafana, and GitHub Actions to show a realistic DevOps workflow.

## Why it was built
The project exists to demonstrate:
- service decomposition and microservice communication
- service discovery and centralized configuration
- container-based deployment
- monitoring and observability
- CI/CD automation with GitHub Actions

## Problem it solves
The original PetClinic app was a single application. This version shows how the same domain can be decomposed into smaller services that can be deployed, scaled, and maintained independently.

## How the project solves it
The solution is organized around microservices that each own a specific domain concern:
- Customers service handles owner/customer data.
- Vets service handles veterinarian information.
- Visits service manages pet visits.
- GenAI service adds conversational AI capabilities.
- API Gateway exposes a unified entry point for clients.
- Config Server centralizes configuration.
- Discovery Server provides service registration and lookup.

## End-to-end flow
1. A client sends a request to the API Gateway.
2. The gateway routes the request to the correct backend service.
3. The service uses configuration from the Config Server and discovers peer services through Eureka.
4. Application metrics are emitted to Prometheus and visualized in Grafana.
5. GitHub Actions builds, tests, and prepares the project for deployment whenever changes are pushed.

## Major components
### Front door
- API Gateway: routes requests to services and provides a single public endpoint.

### Core services
- Customers Service
- Vets Service
- Visits Service
- GenAI Service

### Platform services
- Config Server: provides shared configuration.
- Discovery Server: provides service registration.
- Admin Server: exposes operational monitoring views.
- Zipkin: collects tracing information.

### Observability
- Prometheus collects metrics.
- Grafana visualizes them.

## Container and deployment model
The project uses Docker Compose to orchestrate the stack. The repository also contains Dockerfiles for each service so the application can be packaged consistently for deployment.

## CI/CD workflow
The GitHub Actions workflow performs:
- checkout
- Java 17 setup
- Maven test execution
- Maven packaging
- Docker image build
- optional Docker Hub publishing when secrets are configured
- Trivy vulnerability scanning

## Security and operations
The repository includes:
- a Docker ignore file for leaner image builds
- example environment variables to avoid hardcoding secrets
- workflow permission scoping and concurrency controls
- monitoring and deployment documentation for production use

## Production readiness summary
The repository is now organized to support deployment on a fresh server or EC2 instance. The main remaining operational step is to provide environment variables and any optional publishing credentials before running the stack for the first time.

# Interview Questions for This DevOps Project

## DevOps and CI/CD
1. What does the Maven workflow in this repository validate before deployment?
   - It validates the Java build, test lifecycle, and packaging path used by the microservices. It also prepares the repository for containerization and deployment automation.
2. How are Docker images built and prepared for deployment here?
   - Each service has a dedicated Dockerfile and the workflow builds them from the repository root using Docker Buildx.
3. What is the purpose of the Trivy scan step?
   - Trivy helps surface container image vulnerabilities so that security issues can be reviewed before production deployment.
4. Why is `.dockerignore` important in this project?
   - It keeps image build context smaller and avoids sending unnecessary files such as build outputs, Git metadata, and local IDE files.
5. How would you verify the deployment stack on a fresh EC2 instance?
   - By cloning the repository, setting the environment variables, building the images, starting the stack, and checking the exposed URLs and logs.
6. What is the role of GitHub Actions in this repository?
   - It automates build, test, image creation, and optional image publication workflows for the project.
7. What are the advantages of using concurrency controls in the workflow?
   - They avoid duplicate runs on the same branch and reduce wasted compute during repeated pushes.
8. Why is it useful to scope workflow permissions?
   - It reduces the blast radius and follows the least-privilege principle.

## Architecture
9. What services are part of the PetClinic microservices stack?
   - The stack includes Customers, Vets, Visits, GenAI, API Gateway, Config Server, Discovery Server, Admin Server, and observability services.
10. How does the API Gateway interact with the backend services?
    - It receives incoming traffic and forwards requests to the appropriate downstream microservice.
11. What is the purpose of the Config Server?
    - It centralizes configuration so services do not need hardcoded or duplicated settings.
12. What is the purpose of Eureka?
    - Eureka provides service registration and discovery so the services can locate each other dynamically.
13. How do Prometheus and Grafana work together here?
    - Prometheus collects and stores metrics, while Grafana visualizes them in dashboards.

## Production Readiness
14. What checks should be completed before deploying to production?
    - The environment variables should be set, Docker files should be valid, the workflow should be reviewed, and the application should be reachable through the gateway.
15. Which environment variables are important for this project?
    - The AI-related variables such as OpenAI and Azure OpenAI settings are important for the GenAI service, while the rest of the stack uses the default configuration by convention.
16. Why is a deployment checklist useful?
    - It reduces human error and ensures that each prerequisite and verification step is completed before launch.
17. What is the value of a deployment guide for EC2?
    - It allows a new engineer to perform a fresh deployment without needing to infer the process from the codebase.
18. What would you look for if a service fails to start?
    - The logs, health checks, port conflicts, missing environment variables, and configuration mismatches would be the first things to inspect.
19. How would you make the stack more production-ready?
    - By adding secrets management, stronger health checks, persistent storage where needed, and environment-specific deployment profiles.
20. Why is documentation important for DevOps projects like this one?
    - It shortens onboarding time, improves handoff quality, and reduces the chance of deployment mistakes.

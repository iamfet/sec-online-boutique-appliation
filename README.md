# Secure Online Boutique Application

A comprehensive DevSecOps demonstration project showcasing security-first development practices with automated security scanning, robust CI/CD pipelines, and production-ready deployment strategies. Built around a cloud-native microservices application inspired by [Google's Online Boutique](https://github.com/GoogleCloudPlatform/microservices-demo), this project demonstrates modern DevSecOps best practices using Kubernetes, GitOps, and observability tools.

## ✨ Features

- **Multi-language Microservices**: 11 services built with Go, C#, Node.js, Python, and Java
- **Security-First Development**: Integrated SAST, secrets detection, and vulnerability scanning
- **Advanced Deployment Strategies**: Blue-green and canary deployments
- **Observability**: OpenTelemetry tracing and Google Cloud Profiler integration
- **GitOps Workflow**: Automated deployment via separate GitOps repository
- **Container Security**: Trivy scanning for Docker images
- **Centralized Vulnerability Management**: DefectDojo integration
- **Health Monitoring**: gRPC health checks across all services
- **Load Testing**: Built-in traffic simulation with loadgenerator service

## 🛠️ Tools and Technologies

| Category | Tool/Language | Purpose |
|----------|---------------|----------|
| **Programming Languages** | Go | Frontend, product catalog, checkout, and shipping services |
| | C# | Cart service with .NET Core |
| | Node.js | Payment and currency services |
| | Python | Email, recommendation, load generator, and shopping assistant services |
| | Java | Advertisement service |
| **Security Tools** | SonarQube | Code quality and security analysis |
| | golangci-lint | Go static analysis |
| | Gitleaks | Secrets detection |
| | Snyk | Dependency vulnerability scanning |
| | Trivy | Container image security scanning |
| | DefectDojo | Vulnerability management platform |
| **DevOps & Infrastructure** | Kubernetes | Container orchestration |
| | Docker | Containerization |
| | GitHub Actions | CI/CD pipelines |
| | GitOps | Automated deployment workflows |
| | gRPC | Inter-service communication |
| | Redis | Session storage for cart service |

## 📁 Repository Structure

```
├── src/                    # Microservice source code
│   ├── adservice/         # Java-based ad service
│   ├── cartservice/       # C# shopping cart
│   ├── frontend/          # Go web frontend
│   └── ...
├── kubernetes/            # Kubernetes manifests
│   ├── [service]/deploy.yaml
│   └── k8s-config.yaml   # Complete deployment
├── docker/               # Docker build configurations
└── .github/workflows/    # CI/CD pipelines
```

## 🏗️ Architecture

This application consists of 11 microservices built with different technologies:

| Language | Service | Purpose |
|----------|---------|----------|
| **Go** | frontend | Web UI and API gateway |
| | productcatalogservice | Product inventory management |
| | checkoutservice | Order processing |
| | shippingservice | Shipping quotes and tracking |
| **C#** | cartservice | Shopping cart functionality with .NET Core |
| **Node.js** | paymentservice | Payment processing |
| | currencyservice | Currency conversion |
| **Python** | emailservice | Email notifications |
| | recommendationservice | ML-based product recommendations |
| | loadgenerator | Traffic simulation |
| **Java** | adservice | Advertisement serving |

## 🔒 Security

### Security-First Development Approach
This project implements a comprehensive security strategy that integrates security controls throughout the entire software development lifecycle (SDLC).

### Multi-Layer Security Scanning

#### Static Application Security Testing (SAST)
- **SonarQube**: Comprehensive code quality and security analysis
- **golangci-lint**: Go-specific static analysis with security rules
- **Coverage**: Identifies code quality issues, security hotspots, and vulnerabilities

#### Secrets Detection
- **Gitleaks**: Scans entire repository history for exposed secrets
- **Scope**: API keys, passwords, tokens, and sensitive configuration
- **Prevention**: Blocks commits containing potential secrets

#### Dependency Security
- **Snyk**: Vulnerability scanning for third-party dependencies
- **Coverage**: Known vulnerabilities in npm, pip, Maven, and Go modules
- **Remediation**: Provides fix recommendations and upgrade paths

#### Container Security
- **Trivy**: Comprehensive container image vulnerability scanning
- **Scope**: OS packages, application dependencies, and misconfigurations
- **Integration**: Scans images before registry push

### Vulnerability Management
- **DefectDojo**: Centralized vulnerability management platform
- **Automated Reporting**: All scan results automatically uploaded
- **Risk Assessment**: Vulnerability prioritization and tracking
- **Compliance**: Security findings mapped to compliance frameworks

### Security Pipeline Integration
```
Code Commit → Build & Test → Security Scans → Container Build → Image Scan → Deploy
                                    ↓              ↓
                              DefectDojo ← ← ← ← ← ←
                                    ↓
                            Risk Assessment
                                    ↓
                          Compliance Reporting
```

### Security Best Practices
- **Shift-Left Security**: Security testing integrated early in development
- **Parallel Scanning**: Multiple security tools run concurrently
- **Non-Blocking**: Security scans don't halt development workflow
- **Continuous Monitoring**: Ongoing security assessment in production
- **SARIF Standards**: Standardized security findings format
- **Zero-Trust Architecture**: Service-to-service authentication via mTLS

## 🚀 Deployment

### Prerequisites

| Requirement | Specification |
|-------------|---------------|
| **Kubernetes Cluster** | v1.20+ with RBAC enabled |
| **Docker Registry** | Access to Docker Hub or private registry |
| **Required Secrets** | All pipeline secrets configured in GitHub |
| **Resource Requirements** | Minimum 4 CPU cores and 8GB RAM |
| **Network** | LoadBalancer or Ingress controller for external access |

### Quick Start
```bash
# Deploy all services
kubectl apply -f kubernetes/k8s-config.yaml

# Verify deployment
kubectl get pods -l app

# Access the application
kubectl port-forward svc/frontend 8080:80
```

### Individual Service Deployment
```bash
# Deploy specific service
kubectl apply -f kubernetes/[service-name]/deploy.yaml

# Check service status
kubectl get deployment [service-name]
kubectl get service [service-name]
```

### Advanced Deployment Strategies

#### Blue-Green Deployment
```bash
# Deploy blue-green setup for frontend
kubectl apply -f kubernetes/frontend/blue-green-deployment.yaml

# Switch traffic between versions
kubectl patch service frontend -p '{"spec":{"selector":{"version":"green"}}}'
```

#### Canary Deployment
```bash
# Deploy canary version
kubectl apply -f kubernetes/frontend/canary-deployment.yaml

# Monitor canary metrics before full rollout
kubectl get pods -l version=canary
```

### Production Considerations

| Consideration | Implementation |
|---------------|----------------|
| **Resource Limits** | Configure appropriate CPU/memory limits |
| **Health Checks** | Ensure liveness and readiness probes are configured |
| **Persistent Storage** | Configure volumes for stateful services |
| **Network Policies** | Implement pod-to-pod communication restrictions |
| **Monitoring** | Deploy observability stack via GitOps repository |

## 🔄 CI/CD Pipeline

The GitHub Actions pipeline implements a comprehensive DevSecOps workflow with parallel security scanning and automated deployment. Each service has its own dedicated pipeline that triggers on pull requests to the main branch.

### Pipeline Stages

1. **Build & Test** - Language-specific compilation and unit testing
2. **Code Quality** - Static analysis with golangci-lint for Go services
3. **Security Scanning** - Parallel execution of multiple security tools:
   - **Gitleaks**: Secret detection across the entire repository
   - **Snyk**: Dependency vulnerability scanning
   - **SonarQube**: Code quality and security analysis
4. **Container Build & Scan** - Docker image creation with Trivy security scanning
5. **Manifest Update** - Automated Kubernetes deployment manifest updates
6. **GitOps Trigger** - Repository dispatch to trigger GitOps deployment

### Pipeline Features
- **Parallel Execution**: Security scans run concurrently for faster feedback
- **Continue on Error**: Security scans marked as `continue-on-error: true` to prevent blocking
- **SARIF Integration**: Security findings uploaded to GitHub Security tab
- **DefectDojo Integration**: All scan results automatically uploaded to vulnerability management platform
- **Conditional Deployment**: Container build only proceeds if security scans complete

### Required Secrets

#### Container Registry
- **`DOCKERHUB_USERNAME`** - Docker Hub registry username for image publishing
- **`DOCKERHUB_TOKEN`** - Docker Hub access token for authentication

#### Security Scanning
- **`SNYK_TOKEN`** - Snyk API token for dependency vulnerability scanning
- **`SONAR_TOKEN`** - SonarQube authentication token for code analysis
- **`SONAR_URL`** - SonarQube server URL for API calls

#### Vulnerability Management
- **`DEFECTDOJO_URL`** - DefectDojo instance URL for vulnerability management
- **`DEFECTDOJO_TOKEN`** - DefectDojo API token for scan result uploads
- **`DEFECTDOJO_ENGAGEMENT_ID`** - DefectDojo engagement ID for organizing scan results

#### Git Operations
- **`ACTIONS_PA_TOKEN`** - GitHub Personal Access Token for repository operations
- **`USER_EMAIL`** - Git user email for automated commits
- **`USER_NAME`** - Git username for automated commits

### Pipeline Example: Product Catalog Service
Triggered on pull requests affecting `src/productcatalogservice/`:
- **Build**: Compile with Go 1.22 and run unit tests with coverage
- **Security**: Parallel execution of golangci-lint, Gitleaks, Snyk, and SonarQube
- **Container**: Build Docker image and scan with Trivy for vulnerabilities
- **Deploy**: Update Kubernetes manifests with new image tag
- **GitOps**: Trigger deployment via repository dispatch to GitOps repo

## 🛠️ Development

### Local Development
```bash
# Navigate to specific service
cd src/[service-name]
# Follow service-specific README for setup and running
```

### Pre-commit Security Scanning

#### Gitleaks Pre-commit Hook
Configure Gitleaks as a pre-commit hook to automatically block commits containing secrets.

#### Setup Pre-commit Hook
Create executable pre-commit hook in `.git/hooks/pre-commit`:

```bash
#!/bin/bash
# Gitleaks pre-commit hook - commits are automatically blocked if secrets found
path_to_host_folder_to_scan=$(pwd)
docker run --rm -v ${path_to_host_folder_to_scan}:/path zricethezav/gitleaks:latest detect --source="/path" --verbose
```

```bash
# Make the hook executable
chmod +x .git/hooks/pre-commit
```

## 🔧 Configuration

### Security Integration
- **DefectDojo**: Centralized vulnerability management and reporting
- **Automated Uploads**: Security scan results automatically sent to DefectDojo
- **SARIF Reports**: Standardized security findings format for GitHub Security tab

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make changes following security best practices
4. Ensure all security scans pass
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

This project is built upon the source code from [Google's Online Boutique](https://github.com/GoogleCloudPlatform/microservices-demo), which provides the foundational microservices architecture. We extend our gratitude to Google Cloud Platform for creating and maintaining this excellent demonstration application.

## 🔗 Related Projects

- [Infrastructure Repository](https://github.com/iamfet/sec-eks-infra-automation/) - EKS infrastructure automation
- [GitOps Repository](https://github.com/iamfet/sec-gitops-online-boutique) - Deployment configurations
- [Google's Online Boutique](https://github.com/GoogleCloudPlatform/microservices-demo) - Original upstream project

---

**Built with ❤️ for DevSecOps demonstrations**
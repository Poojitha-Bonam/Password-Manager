# P3 Development Plan - Detailed Todo List

**Project:** End-to-End Modernization of Password Manager Monolithic Secure Vault to Cloud-Native Microservices  
**Duration:** 15-18 weeks

---

## Pre-Phase 0: Baseline Cleanup and Alignment (1 week)

- [ ] **7.1 Runtime and dependency alignment**
  - [ ] Align backend Dockerfile, Maven compiler version
  - [ ] Align frontend Node version
  - [ ] Create agreed runtime matrix
  - [ ] Update README setup instructions

- [ ] **7.2 Secrets and config cleanup**
  - [ ] Remove hardcoded database secrets from source
  - [ ] Remove hardcoded JWT secrets from source
  - [ ] Remove hardcoded mail secrets from source
  - [ ] Move secrets to environment variables
  - [ ] Create .env.example
  - [ ] Standardize config profiles (local, docker, aws)
  - [ ] Document config matrix

- [ ] **7.3 Baseline testing**
  - [ ] Create smoke test checklist for critical flows
  - [ ] Ensure backend tests pass consistently
  - [ ] Ensure frontend tests are runnable
  - [ ] Test critical flows: register, login, refresh token, 2FA setup
  - [ ] Test critical flows: create vault entry, view sensitive entry
  - [ ] Test critical flows: generate password, export backup
  - [ ] Test critical flows: create and read notification
  - [ ] Document smoke test results
  - [ ] Establish stable baseline

---

## Phase 1: Containerization, CI/CD, and AWS Deployment (4-5 weeks)

### Phase 1.1: Container-ready monolith
- [ ] **1.1.1 Backend Docker setup**
  - [ ] Finalize backend multi-stage Dockerfile
  - [ ] Test backend Docker build
  - [ ] Verify health checks

- [ ] **1.1.2 Frontend Docker setup**
  - [ ] Finalize frontend multi-stage Dockerfile
  - [ ] Test frontend Docker build
  - [ ] Configure Nginx for production

- [ ] **1.1.3 Docker Compose**
  - [ ] Create Docker Compose for mysql
  - [ ] Create Docker Compose for backend
  - [ ] Create Docker Compose for frontend
  - [ ] Verify file storage mounting
  - [ ] Verify logs volume mounting
  - [ ] Test full application in containers

### Phase 1.2: AWS infrastructure setup
- [ ] **1.2.1 Network setup**
  - [ ] Provision VPC
  - [ ] Provision subnets (public/private)
  - [ ] Provision security groups

- [ ] **1.2.2 Database setup**
  - [ ] Provision RDS MySQL Multi-AZ
  - [ ] Configure database credentials
  - [ ] Test database connectivity

- [ ] **1.2.3 Container registry**
  - [ ] Provision ECR repositories
  - [ ] Push test images to ECR

- [ ] **1.2.4 Compute and load balancing**
  - [ ] Provision EC2 Auto Scaling Group
  - [ ] Provision Application Load Balancer
  - [ ] Configure target groups
  - [ ] Register backend instances

### Phase 1.3: Secrets and cloud configuration
- [ ] **1.3.1 Secrets Manager**
  - [ ] Create AWS Secrets Manager entries
  - [ ] Database credentials
  - [ ] JWT secrets
  - [ ] Mail credentials

- [ ] **1.3.2 Spring configuration**
  - [ ] Wire config to load from environment
  - [ ] Wire config to load from AWS Secrets Manager
  - [ ] Separate local config from AWS config
  - [ ] Test cloud config loading

- [ ] **1.3.3 Documentation**
  - [ ] Create secret rotation checklist
  - [ ] Document deployment config

### Phase 1.4: Jenkins CI/CD pipeline
- [ ] **1.4.1 Pipeline stages**
  - [ ] Create Jenkinsfile
  - [ ] Configure Git checkout stage
  - [ ] Configure Maven build stage
  - [ ] Configure backend test stage
  - [ ] Configure SonarQube analysis
  - [ ] Configure Docker image build
  - [ ] Configure image push to ECR
  - [ ] Configure deployment to EC2/ASG

- [ ] **1.4.2 Quality gates**
  - [ ] Configure build fail on test failure
  - [ ] Configure build fail on image build failure
  - [ ] Configure deployment only after successful build

- [ ] **1.4.3 Pipeline testing**
  - [ ] Test full CI/CD flow
  - [ ] Verify push to main triggers pipeline

### Phase 1.5: Production hardening and validation
- [ ] **1.5.1 Health checks**
  - [ ] Add Actuator health endpoints
  - [ ] Verify ALB target health
  - [ ] Configure health check intervals

- [ ] **1.5.2 Logging and monitoring**
  - [ ] Verify logs are collected
  - [ ] Configure log aggregation

- [ ] **1.5.3 Validation**
  - [ ] Run smoke tests against AWS
  - [ ] Document rollback process
  - [ ] Create deployment validation report

---

## Phase 2: Microservices Transformation and Local Kubernetes Orchestration

### Phase 2A: Prepare Monolith for Extraction (2 weeks)

- [ ] **2A.1 Identify bounded contexts**
  - [ ] Map controllers to target services
  - [ ] Map entities to target services
  - [ ] Map repositories to target services
  - [ ] Document cross-boundary dependencies
  - [ ] Create service mapping table

- [ ] **2A.2 Refactor for clean boundaries**
  - [ ] Create internal facade interfaces
  - [ ] Reduce cross-domain repository access
  - [ ] Isolate crypto operations under vault-owned code
  - [ ] Isolate email sending under notification-owned code
  - [ ] Isolate auth/session logic under user-owned code

- [ ] **2A.3 Stabilize API contracts**
  - [ ] List exact current public routes
  - [ ] Freeze external route names
  - [ ] Generate/clean up OpenAPI contract
  - [ ] Regenerate Angular API client

### Phase 2B: Build Shared Spring Cloud Platform (1-2 weeks)

- [ ] **2B.1 API Gateway**
  - [ ] Create Spring Cloud Gateway project
  - [ ] Define route mappings for existing API paths
  - [ ] Add CORS configuration
  - [ ] Add request logging
  - [ ] Add Resilience4j circuit breaker
  - [ ] Add edge rate limiting
  - [ ] Test gateway routing to monolith

- [ ] **2B.2 Eureka Server**
  - [ ] Create Eureka Server project
  - [ ] Configure Eureka settings
  - [ ] Register sample service
  - [ ] Verify service discovery flow

- [ ] **2B.3 Config Server**
  - [ ] Create Config Server project
  - [ ] Move service configs to centralized config repo
  - [ ] Define per-service profiles (local, minikube, test)
  - [ ] Test centralized config loading

- [ ] **2B.4 Shared service template**
  - [ ] Create base service template
  - [ ] Add actuator configuration
  - [ ] Add logging configuration
  - [ ] Add OpenAPI support
  - [ ] Add resilience configuration
  - [ ] Add security config pattern
  - [ ] Add test template
  - [ ] Document template usage

### Phase 2C: Extract Services (5-7 weeks)

#### Phase 2C.1: Notification Service extraction (Week 1)
- [ ] **2C.1.1 Code extraction**
  - [ ] Move notification controller
  - [ ] Move notification service
  - [ ] Move notification repository
  - [ ] Move notification model/entity
  - [ ] Move email service (if notification-owned)

- [ ] **2C.1.2 Configuration**
  - [ ] Create database schema for notifications
  - [ ] Configure routes (/api/notifications/**)
  - [ ] Connect to Config Server

- [ ] **2C.1.3 Integration**
  - [ ] Connect frontend notification bell to gateway
  - [ ] Test notification polling
  - [ ] Test mark-read flows through gateway

#### Phase 2C.2: User Service extraction (Week 2-3)
- [ ] **2C.2.1 Code extraction**
  - [ ] Move auth controllers
  - [ ] Move user controllers
  - [ ] Move 2FA controllers
  - [ ] Move session controllers
  - [ ] Move authentication services
  - [ ] Move JWT issuance logic

- [ ] **2C.2.2 Data migration**
  - [ ] Move user entities
  - [ ] Move user repositories
  - [ ] Create user database schema

- [ ] **2C.2.3 Configuration**
  - [ ] Configure routes (/api/auth/**, /api/users/**, /api/2fa/**, /api/sessions/**)
  - [ ] Connect to Config Server

- [ ] **2C.2.4 Integration**
  - [ ] Create Feign clients where needed
  - [ ] Test login/logout through gateway
  - [ ] Test refresh token
  - [ ] Test profile and settings
  - [ ] Test 2FA setup and verification

#### Phase 2C.3: Vault Service extraction (Week 3-4)
- [ ] **2C.3.1 Code extraction**
  - [ ] Move vault controllers
  - [ ] Move vault services
  - [ ] Move category controllers
  - [ ] Move folder controllers
  - [ ] Move timeline controllers

- [ ] **2C.3.2 Data migration**
  - [ ] Move vault entities
  - [ ] Move category entities
  - [ ] Move folder entities
  - [ ] Move vault repositories
  - [ ] Create vault database schema

- [ ] **2C.3.3 Configuration**
  - [ ] Configure routes (/api/vault/**, /api/categories/**, /api/folders/**)
  - [ ] Isolate encryption logic
  - [ ] Connect to Config Server

- [ ] **2C.3.4 Integration**
  - [ ] Connect frontend vault screens to gateway
  - [ ] Test vault CRUD operations
  - [ ] Test categories and folders
  - [ ] Test sensitive view
  - [ ] Test trash and snapshots

#### Phase 2C.4: Security Service extraction (Week 5)
- [ ] **2C.4.1 Code extraction**
  - [ ] Move security controllers
  - [ ] Move dashboard controllers
  - [ ] Move audit services
  - [ ] Move alert services
  - [ ] Move password analysis services

- [ ] **2C.4.2 Data migration**
  - [ ] Move audit entities
  - [ ] Move alert entities
  - [ ] Move login attempt entities
  - [ ] Move password analysis entities
  - [ ] Create security database schema

- [ ] **2C.4.3 Configuration**
  - [ ] Configure routes (/api/security/**, /api/dashboard/**)
  - [ ] Connect to Config Server

- [ ] **2C.4.4 Integration**
  - [ ] Connect frontend dashboard to gateway
  - [ ] Test dashboard metrics
  - [ ] Test audit logs
  - [ ] Test login history

#### Phase 2C.5: Generator Service extraction (Week 6)
- [ ] **2C.5.1 Code extraction**
  - [ ] Move generator controller
  - [ ] Move generator services

- [ ] **2C.5.2 Configuration**
  - [ ] Configure routes (/api/generator/**)
  - [ ] Connect to Config Server

- [ ] **2C.5.3 Integration**
  - [ ] Connect frontend generator to gateway
  - [ ] Test password generation
  - [ ] Test strength checking

### Phase 2C-Extended: Optional additional business service splits (1-3 weeks)

#### Phase 2C-Extended.1: Sharing Service extraction
- [ ] Move secure share controller from Vault
- [ ] Move secure share service
- [ ] Create secure_shares ownership
- [ ] Configure routes (/api/shares/**)
- [ ] Connect to Vault via Feign

#### Phase 2C-Extended.2: Analytics Service extraction
- [ ] Move dashboard logic from Security
- [ ] Move timeline logic
- [ ] Create read-model style endpoints
- [ ] Configure routes (/api/dashboard/**, /api/timeline/**)
- [ ] Connect to User, Vault, Security via Feign

### Phase 2D: Local Kubernetes and Observability (2 weeks)

#### Phase 2D.1: Containerize every microservice
- [ ] Create Dockerfile for User Service
- [ ] Create Dockerfile for Vault Service
- [ ] Create Dockerfile for Generator Service
- [ ] Create Dockerfile for Security Service
- [ ] Create Dockerfile for Notification Service
- [ ] Create Dockerfile for API Gateway
- [ ] Create Dockerfile for Eureka Server
- [ ] Create Dockerfile for Config Server
- [ ] Add service-level health checks
- [ ] Verify configuration via Config Server

#### Phase 2D.2: Minikube deployment
- [ ] Create Deployment manifests
- [ ] Create Service manifests
- [ ] Create ConfigMaps
- [ ] Create Secrets
- [ ] Create Ingress
- [ ] Create HPA for main services
- [ ] Test all services on Minikube

#### Phase 2D.3: Logging, monitoring, and tracing
- [ ] **ELK Stack**
  - [ ] Deploy Elasticsearch
  - [ ] Deploy Logstash
  - [ ] Deploy Kibana
  - [ ] Configure log aggregation

- [ ] **Prometheus + Grafana**
  - [ ] Deploy Prometheus
  - [ ] Expose scrape targets
  - [ ] Deploy Grafana
  - [ ] Build dashboards

- [ ] **Distributed tracing**
  - [ ] Deploy Jaeger
  - [ ] Add tracing headers
  - [ ] Configure trace visualization

#### Phase 2D.4: Final hardening and demo preparation
- [ ] Run full smoke tests on Minikube
- [ ] Verify gateway routing
- [ ] Verify service registration
- [ ] Verify config loading
- [ ] Verify fault tolerance behavior
- [ ] Verify dashboards and logs
- [ ] Prepare demo scripts
- [ ] Prepare architecture diagrams
- [ ] Create deployment checklist
- [ ] Final demo-ready platform

---

## Summary

| Phase | Duration | Status |
|-------|----------|--------|
| Pre-Phase 0 | 1 week | [x] |
| Phase 1 | 4-5 weeks | [ ] |
| Phase 2A | 2 weeks | [ ] |
| Phase 2B | 1-2 weeks | [ ] |
| Phase 2C | 5-7 weeks | [ ] |
| Phase 2C-Extended | 1-3 weeks | [ ] |
| Phase 2D | 2 weeks | [ ] |

**Total: 15-18 weeks**

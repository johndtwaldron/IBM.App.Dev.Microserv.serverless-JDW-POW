# IBM App Dev with Microservices & Serverless — Notes (JDW)

Course: https://www.coursera.org/learn/applications-development-microservices-serverless-openshift/home/info

This file is my **personal resource summary** from the IBM DevOps course and final project.  
It consolidates module summaries, quiz answers, CLI commands, and key concepts.

---

## 🧩 Module 1: Introduction to Microservices

- Modern dev delivers centrally hosted SaaS apps.  
- **12-Factor Methodology** → maps to *Code, Deploy, Operate*.  
- Microservices = independent, loosely coupled, deployable smaller services.  
- Each service can use a different tech stack.  
- Scaling is independent; failures isolated.  
- Anti-patterns to avoid: tight coupling, single DB for all, shared state.

**Key terms**  
- **BFF**: Backend for Frontend  
- **Strangler pattern**: refactor monolith in stages  
- **SPA**: Single Page Application  
- **Service discovery**: find services automatically  

---

## 🧩 Module 2: Web API Essentials (REST API & GraphQL)

- REST = stateless, scalable, CRUD over HTTP (GET, POST, PUT, DELETE).  
- **API Gateway**: entry point to backend, adds auth/logging.  
- **Tools**: Flask (Python micro-framework), Postman, Swagger/OpenAPI, cURL.  
- GraphQL: query language → return exactly what you need.  

**Common cURL**  
```bash
# Query products
curl -X GET 'http://127.0.0.1:5000/products' -H 'accept: application/json'

## 🧩 Module 3: Serverless Overview

- Serverless: build/run apps without managing servers.

- Built-in high availability, fault tolerance, pay-per-request.

- Constraints: latency, debugging complexity, no state persistence.

- FaaS: run functions on events (subset of serverless).

- BaaS: backend as a service (DBs, auth, storage).

- Stacks: AWS Lambda, GCP Functions, Azure Functions, IBM Cloud Functions.

## 🧩 Module 4: Create & Deploy Microservices

IBM Cloud Code Engine: fully managed → apps, builds, jobs.

Deploy from source or container image.

Docker: build images using Dockerfile.

Buildpacks: auto-detect source, create build plan.

Typical Dockerfile start:

FROM python:3.9
COPY . /app
CMD ["python", "app.py"]

CLI example:

ibmcloud ce app create --name helloworld \
  --image us.icr.io/mynamespace/hello_repo \
  --registry-secret myregistry

## 🧩 Optional: OpenShift & Service Mesh

OpenShift = Kubernetes distribution + extra tools.

Automates build pipelines, pushes to registry.

Service mesh (Istio): provides traffic shifting, mTLS security, telemetry.

## 🛠️ Final Project Workflow
Deploy Product Details (Python, port 5000)
ibmcloud ce app create \
  --name prodlist \
  --image us.icr.io/${SN_ICR_NAMESPACE}/prodlist \
  --registry-secret icr-secret \
  --port 5000 \
  --build-context-dir products_list \
  --build-source https://github.com/ibm-developer-skills-network/dealer_evaluation_backend.git

Deploy Dealer Pricing (Node.js, port 8080)
ibmcloud ce app create \
  --name dealerdetails \
  --image us.icr.io/${SN_ICR_NAMESPACE}/dealerdetails \
  --registry-secret icr-secret \
  --port 8080 \
  --build-context-dir dealer_details \
  --build-source https://github.com/ibm-developer-skills-network/dealer_evaluation_backend.git

Deploy Frontend (port 5001)
ibmcloud ce app create \
  --name frontend \
  --image us.icr.io/${SN_ICR_NAMESPACE}/frontend \
  --registry-secret icr-secret \
  --port 5001 \
  --build-source .


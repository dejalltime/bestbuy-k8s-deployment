# Best Buy Cloud-Native Application

A full-stack cloud-native microservices application for Best Buy, deployed to Azure Kubernetes Service (AKS). This app demonstrates microservices, a managed order queue using Azure Service Bus, and AI integration via OpenAI.

---

## Architecture Overview

- **Frontend**
  - `store-front`: Customer-facing UI
  - `store-admin`: Admin-facing UI

- **Backend Microservices**
  - `order-service`: Handles orders and sends to Azure Service Bus
  - `product-service`: Manages product data
  - `makeline-service`: Consumes queue messages and fulfills orders

- **AI Integration**
  - `ai-service`: Generates product descriptions & images using GPT-4 + DALL·E

- **Database**
  - MongoDB for orders & products

- **Queue**
  - Azure Service Bus (replaces RabbitMQ)

---

## Deployment Instructions

### 1. Clone all microservices
Each service has its own repository and Dockerfile. Clone and modify `.env` as needed.

### 2. Docker Compose (Local Testing)

```bash
docker-compose up --build
```

### 3. Push Docker Images

```bash
docker build -t <your-dockerhub-username>/<service-name>:latest .
docker push <your-dockerhub-username>/<service-name>:latest
```

### 4. Create Azure Resources

```bash
az group create --name BestBuyRG --location canadacentral
az aks create --resource-group BestBuyRG --name BestBuyCluster --node-count 2 --enable-addons monitoring --generate-ssh-keys
az aks get-credentials --resource-group BestBuyRG --name BestBuyCluster
```

### 5. Apply Secrets and ConfigMaps

```bash
kubectl apply -f secrets.yaml
kubectl apply -f configmap.yaml
```

### 6. Deploy Services

```bash
kubectl apply -f DeploymentFiles/
```

---

## Project Structure

```
.
├── DeploymentFiles/
│   ├── store-front.yaml
│   ├── store-admin.yaml
│   ├── order-service.yaml
│   ├── product-service.yaml
│   ├── makeline-service.yaml
│   ├── mongodb.yaml
│   ├── secrets.yaml
│   └── configmap.yaml
├── README.md
```

---

## Secrets & Config

Use `secrets.yaml` for:

- ORDER_QUEUE_URI
- ORDER_QUEUE_HOSTNAME
- ORDER_QUEUE_USERNAME
- ORDER_QUEUE_PASSWORD
- ORDER_QUEUE_NAME
- ORDER_QUEUE_PORT
- ORDER_QUEUE_TRANSPORT
- ORDER_DB_URI
- ORDER_DB_NAME
- ORDER_DB_COLLECTION_NAME

Use `configmap.yaml` for:

- USE_WORKLOAD_IDENTITY_AUTH
- APP_NAME
- MONGODB_SERVICE_NAME (if needed)

---

## Demo Video

[Watch on YouTube](https://youtube.com/your-demo-link)

---

## 📦 Microservice Repos

| Service          | GitHub Repo Link                          |
|------------------|--------------------------------------------|
| Store-Front      | https://github.com/dejalltime/store-front        |
| Store-Admin      | https://github.com/dejalltime/store-admin        |
| Order-Service    | https://github.com/dejalltime/order-service      |
| Product-Service  | https://github.com/dejalltime/product-service    |
| Makeline-Service | https://github.com/dejalltime/makeline-service   |

---

## 🐳 Docker Images

| Service          | DockerHub Link                              |
|------------------|----------------------------------------------|
| Store-Front      | https://hub.docker.com/r/dejmartins/store-front    |
| Store-Admin      | https://hub.docker.com/r/dejmartins/store-admin    |
| Order-Service    | https://hub.docker.com/r/dejmartins/order-service  |
| Product-Service  | https://hub.docker.com/r/dejmartins/product-service |
| Makeline-Service | https://hub.docker.com/r/dejmartins/makeline-service |

---

## Status

- [x] Microservices Dockerized
- [x] Azure Service Bus Integrated
- [x] Secrets managed via K8s
- [x] Deployed to AKS
- [x] AI service with OpenAI ready

---
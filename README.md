# Project Assignment #2: Cloud-Native App Best-Buy Application

This repository includes the source code, documentation, and deployment files for a cloud-native demo application modeled after Best Buy. The application features a microservices architecture and integrates AI capabilities for generating product descriptions and images.
## Application Architecture

### Application Table

| Service              | Description                                                       | Notes                           |
| -------------------- | ----------------------------------------------------------------- | ------------------------------- |
| **Store-Front**      | Customer-facing app for browsing and placing orders              | -                               |
| **Store-Admin**      | Employee-facing app for managing products and viewing orders .    | -                               |
| **Order-Service**    | Handles order creation and sends data to the managed order queue. | Uses RabbitMQ for messaging.    |
| **Product-Service**  | Handles CRUD operations for product data.                         | -                               |
| **Makeline-Service** | Processes and completes orders by reading from the order queue.   | -                               |
| **AI-Service**       | Generates product descriptions and images.                        | Uses GPT-4 and DALL-E-3 models. |
| **Database**         | MongoDB for persisting order and product data.                    | -                               |


### Diagram

![Algonquin Pet Store On Steroids](https://github.com/user-attachments/assets/ca65182d-4497-4cb0-9125-ce32c821e4aa)

### Architecture Overview

**Store-Front**: A React.js-based microservice that enables customers to browse products and place orders.

**Store-Admin**: An administrative portal for employees to manage inventory and track customer orders.

**Order-Service**: Handles order processing and sends order data to Azure Service Bus.

**Product-Service**: Manages product data, interacting with the database and AI services.

**Makeline-Service**: Retrieves order details from Azure Service Bus, processes them, and updates order statuses.

**AI-Service**:  Connects with OpenAI APIs to dynamically create product descriptions and images.

**MongoDB**:Serves as the database for storing product and order details.


### Deployment Instructions

### Prerequisites

* Azure account for Azure Service Bus and Azure Kubernetes Service (AKS).

* Docker installed and configured.

* kubectl CLI installed and configured for your AKS cluster.

Access to OpenAI APIs for GPT-4 and DALL-E.

Steps

1. Clone the Repository:
```
 git clone https://github.com/shivareddysama/best-buy
 ```
```
 cd best-buy-app
```
2. Build and Push Docker Images:

   For each microservice, navigate to its directory and run:
```
docker build -t shivareddysama/<service-name>:laatest .
docker push shivareddysama/<service-name>:latest
```
3. Setup Azure Resources

Resource group name: assignment

Region: East US.

Click Review + Create and then Create.

4. Create AKS Clusters

- Select **agentpool**. This nodes will have the controlplane.
        - Set **node size** to `D2as_v4`.
        - **Scale method**: `Manual`
        - **Node count**: `1`
        - Click `update`
     - Click on **Add node pool**:
        - **Node pool name**: `workernodes`.
        - **Mode**: `User` 
        - Set **node size** to `D2as_v3`.
        - **Scale method**: `Manual`
        - **Node count**: `2`
        - Click `add`
   - Click **Review + Create**, and then **Create**. The deployment will take a few minutes.

**Deploy to Kubernetes** Apply the deployment YAML files:
```
kubectl apply -f Deployment Files/
 ```

4. Verify Deployment:
   Ensure all pods are running
```
kubectl get pods
```
5. Deploy Microservices

Apply the deployment YAML files for each microservice:

```
kubectl apply -f Deployment_Files/store-front.yaml -n best-buy-app
kubectl apply -f Deployment_Files/store-admin.yaml -n best-buy-app
kubectl apply -f Deployment_Files/order-service.yaml -n best-buy-app
kubectl apply -f Deployment_Files/product-service.yaml -n best-buy-app
kubectl apply -f Deployment_Files/makeline-service.yaml -n best-buy-app
kubectl apply -f Deployment_Files/ai-service.yaml -n best-buy-app

```
### Issues and Limitations
**Azure Service Bus Integration**
   - Replacing RabbitMQ required significant code for message handling.
**Azure AI Service**
   - Not able to fix the AI service.


## Repositories
This table lists the GitHub repositories for each of the microservices in the project:

| **Service**        | **Repository Link**                          |
|--------------------|----------------------------------------------|
| Store-Front        | https://github.com/shivareddysama/store-front-final |
| Order-Service      |https://github.com/shivareddysama/order-service-final |
| Product-Service    |https://github.com/shivareddysama/product-service-final |
| Makeline-Service   | https://github.com/shivareddysama/makeline-service-final |
| Store-Admin        | https://github.com/shivareddysama/store-admin-final |
| AI-Service         | https://github.com/shivareddysama/ai-service-final |

## Docker Images
This table lists the docker images for each of the microservices in the project:

| **Service**        | **Docker Image Link**                          |
|--------------------|----------------------------------------------|
| Store-Front        | https://hub.docker.com/repository/docker/shivareddysama/store-front-final/general |
| Order-Service      | https://hub.docker.com/repository/docker/shivareddysama/order-service-final/general |
| Product-Service    | https://hub.docker.com/repository/docker/shivareddysama/product-service-final/general |
| Makeline-Service   |https://hub.docker.com/repository/docker/shivareddysama/makeline-service-final/general |
| Store-Admin        | https://hub.docker.com/repository/docker/shivareddysama/store-admin-final/general |
| AI-Service         | https://hub.docker.com/repository/docker/shivareddysama/ai-service-final/general |

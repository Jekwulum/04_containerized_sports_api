# 04_containerized_sports_api

## steps
1. Clone the repository
  ```shell
    git clone git@github.com:Jekwulum/04_containerized_sports_api.git
    cd 04_containerized_sports_api
  ```
2. Login to Azure CLI
  ```shell
    az login
  ```
3. Create Azure Container Registry (ACR)
  ```shell
    az acr create --resource-group <ResourceGroupName> --name <ACRName> --sku Basic
  ```
4. Login to ACR
  ```shell
    az acr login --name <ACRName>
  ```
5. Build, and Push the Docker Image
  ```shell
    docker build -t <ACRName>.azurecr.io/sports-api .
    docker push <ACRName>.azurecr.io/sports-api:latest
    # when changes are made to the files, perform these two operations again
  ```
6. Set Up Azure a Container App
  ```shell
    az containerapp create \
    --name sports-api-app \
    --resource-group <ResourceGroupName> \
    --image <ACRName>.azurecr.io/sports-api \
    --environment-variables SPORTS_API_KEY=<YOUR_SPORTSDATA.IO_API_KEY> \
    --target-port 8080 \
    --ingress external

    # when prompted to login, obtain the username and password from the portal in the container registry Access Keys after selecting "admin user"
  ```
7. Create an API Management Instance
  ```shell
   az apim create --name <APIMName> --resource-group <ResourceGroupName> --publisher-name <PublisherName> --publisher-email <PublisherEmail>
   
   # Check the status of the deployment by running the az apim show command
   az apim show --name <APIMName> --resource-group <ResourceGroupName> --output table
  ```
8. 
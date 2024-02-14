pipeline {
    agent any

    environment {
        AZURE_SUBSCRIPTION_ID = 'c8ce3edc-0522-48a3-b7e4-afe8e3d731d9'
        AZURE_TENANT_ID = '4ccd6048-181f-43a0-ba5a-7f48e8a4fa35'
        CONTAINER_REGISTRY = 'goodbirdacr.azurecr.io'
        RESOURCE_GROUP = 'AKS'
        REPO = 'medicine/front'
        IMAGE_NAME = 'medicine/front:latest'
        TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Push Docker Image to ACR') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'acr-credential-id', passwordVariable: 'ACR_PASSWORD', usernameVariable: 'ACR_USERNAME')]) {
                        // Log in to ACR
                        sh "az acr login --name $CONTAINER_REGISTRY --username $ACR_USERNAME --password $ACR_PASSWORD"

                        // Build and push Docker image to ACR
                        // 변경: 이미지 이름을 $CONTAINER_REGISTRY/$IMAGE_NAME으로 수정
                        sh "docker build -t $CONTAINER_REGISTRY/$IMAGE_NAME ."
                        sh "docker push $CONTAINER_REGISTRY/$IMAGE_NAME"
                    }
                }
            }
        }
    }
}
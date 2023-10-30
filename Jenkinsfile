#!/usr/bin/env groovy
pipeline {
    agent any
    environment {
        AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        AWS_DEFAULT_REGION = "us-east-1"
    }
    stages {
        stage("Create an EKS Cluster") {
            steps {
                script {
                    dir('terraform') {
                        sh "terraform init"
                        sh "terraform apply -auto-approve"
                    }
                }
            }
        }
        stage("Deploy to EKS") {
    steps {
        script {
            dir('kubernetes') {
                sh "aws eks update-kubeconfig --name myapp-eks-cluster"
                
                // Apply your YAML files one by one
                sh "kubectl apply -f app-deployment.yaml"
                sh "kubectl apply -f db-deployment.yaml"
                sh "kubectl apply -f mysql-configMap.yaml"
                sh "kubectl apply -f mysql-secrets.yaml"
            }
        }
    }
}

    }
}

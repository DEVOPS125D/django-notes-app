@Library('Shared') _

pipeline {
    agent { 
        node {
            label 'bilal' 
        }
    }
    
    stages {
        stage("hello") {
            steps {
                hello()
            }
        }

        stage("Code") {
            steps {
                echo "Cloning code from personal repository"
                git url: "https://github.com/DEVOPS125D/django-notes-app.git", branch: "main"
            }
        }
        
        stage("Build") {
            steps {
                echo "Building Docker image"
                sh "docker build -t notes-app:latest ."
            }
        }
        
        stage("Push to DockerHUB") {
            steps {
                echo "Pushing image to Docker Hub"
                withCredentials([usernamePassword('credentialsId': "dockerHUBCred", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]) {
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker image tag notes-app:latest ${env.dockerHubUser}/notes-app:latest"
                    sh "docker push ${env.dockerHubUser}/notes-app:latest"
                }
            }
        }
        
        stage("Deploy") {
            steps {
                echo "Deploying application containers"
                sh "docker compose down || true"
                sh "docker compose up -d"
            }
        }
    }
}

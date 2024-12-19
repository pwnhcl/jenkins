pipeline {
    agent any
    stages {
        stage("code") {
            steps {
                echo "This is cloning the code"
                git url: "https://github.com/pwnhcl/jenkins.git", branch: "master"
                echo "Code Clonning Successfull" 
            }
        }
        stage("build") {
            steps {
                echo "This is building the code"
                sh "docker build -t frontend frontend:v1 ./mern/frontend"
                sh "docker build -t frontend backend:v1 ./mern/backend"
            }
        }
        stage("push to DockerHub") {
            steps {
                echo "This is pushing the image in dockerHub"
                withCredentials([usernamePassword(credentialsId: 'DockerHubCred'', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]){
                sh "docker login -u ${env.DOCKER_USERNAME} -p ${env.DOCKER_PASSWORD}"
                sh "docker image tag frontend:v1 ${env.DOCKER_USERNAME}/frontend:v1"
                sh "docker image tag backend:v1 ${env.DOCKER_USERNAME}/backend:v1"
                }
            }
        }
        stage("deploy") {
            steps {
                echo "This is deploying the code"
                sh "docker compose up -d"
            }
        }
    }
}

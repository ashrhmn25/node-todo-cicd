pipeline {
    agent any
    stages {
        stage ("Code") {
            steps {
                echo "Code clone ho gya hai"
                git url: "https://github.com/ashrhmn25/node-todo-cicd.git", branch: "master"
            }
        }
        stage ("Build & Test") {
            steps {
                echo "docker build bhi ho gya hai"
                sh "docker build . -t node-app-demo"
            }
        }
        stage ("Push to Repository") {
            steps {
                echo "dockerHub pe push ho gya hai"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag node-app-demo ${env.dockerHubUser}/node-app-demo:latest"
                    sh "docker push ${env.dockerHubUser}/node-app-demo:latest"
                }
            }
        }
        stage ("Deploy") {
            steps {
                echo "Amazon EC2 pe docker compose ne deploy kar diya hai"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}

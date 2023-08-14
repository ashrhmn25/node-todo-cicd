pipeline {
    agent {label 'dev-agent'}
    
    stages{
        stage('Code'){
            steps {
                git url: 'https://github.com/ashrhmn25/node-todo-cicd.git', branch: 'master'
            }
        }
        stage('Build and Test'){
            steps {
                sh "docker build . -t ashraf1980/node-todo-app-cicd:latest"
            }    
        }
        stage('Login and Push Image'){
            steps{
                echo "Login to docker hub and push image.."
                withCredentials([usernamePassword(credentialsId:'dockerHub',passwordVariable:'dockerHubPass',usernameVariable:'dockerHubUser')]) {
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh 'docker push ${env.dockerHubUser}/node-todo-app-cicd:latest'
                }
            }
        }
        stage('Deploy'){
            steps {
                sh 'docker-compose down'
                sh 'docker-compose up -d --no-deps --build web'
            }
        }
    }
}

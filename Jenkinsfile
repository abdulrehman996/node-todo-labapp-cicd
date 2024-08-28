pipeline {
    agent {
        // Replace 'azure-vm-agent' with the name of your Jenkins agent configured for the Azure VM
        label 'Azure VM'
    }
    
    stages{
        stage('code'){
            steps {
                git url: 'https://github.com/abdulrehman996/node-todo-labapp-cicd.git', branch: 'main'
            }
        }
        stage('Build and Test'){
            steps {
                sh 'docker build . -t abdulrehman996/node-todo-labapp-cicd:latest'
            }
        }
        stage('Login and Push Image'){
            steps {
                echo 'logging in to docker hub and pushing image..'
                withCredentials([usernamePassword(credentialsId:'docker-hub-pat',passwordVariable:'DockerHubPassword', usernameVariable:'DockerHubUsername')]) {
                    sh "docker login -u ${env.DockerHubUsername} -p ${env.DockerHubPassword}"
                    sh "docker push abdulrehman996/node-todo-labapp-cicd:latest"
                }    
            }
        }
        stage('Deploy'){
            steps {
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }
}

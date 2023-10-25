pipeline{
    agent {label 'dev-agent'}
    
    stages {
        stage('Code'){
            steps{
                script {
                    properties([pipelineTriggers([pollSCM('')])])
                }
                git url : 'https://github.com/Akshat0891/node-todo-cicd.git', branch : 'master'
            }
        }
        stage('Build and Test'){
            steps{
                sh 'docker build . -t akshat0891/node-todo-app-cicd:latest'
            }
        }
        stage('Login and Push Image to Dockerhub'){
            steps{
                echo 'Logging into Dockerhub and pushing image..'
                withCredentials([usernamePassword(credentialsId:'dockerhub',passwordVariable:'dockerHubPassword', usernameVariable:'dockerHubUser')])
                {
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                    sh "docker push akshat0891/node-todo-app-cicd:latest"
                }    
            }
        }
        stage('Deploy'){
            steps{
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }
}

pipeline{
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials')
    }
    stages{
        stage('Clone Repository'){
            steps{
                git branch: 'main', url: 'https:github.com/cnnaik/todo-application.git'
            }
        }

        stage('Build'){
            steps{
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Image'){
            steps{
                sh 'docker build -t todo-application-image:latest .'
            }
        }

        stage('Push Image to Dockerhub'){
            steps{
                sh 'docker login -u $DOCKER_HUB_CREDENTIALS_USR -p $DOCKER_HUB_CREDENTIALS_PSW'
                sh 'docker tag todo-application-image:latest cnaik/todo-application:latest'
                sh 'docker push cnaik/todo-application:latest'
           }
        }

        stage('Deploy Application'){
            steps{
                sh 'docker compose up -d'
           }
        }

        stage('Verify Deployment'){
            steps{
                sh 'docker ps'
           }
        }

        stage('Clean Workspace'){
            steps{
                sh 'rm -rf *'
           }
        }        
    }

    post {
        success {  
           echo "Build Successfull"
        }
        failure {
           echo "Failure"
        }
    }
}

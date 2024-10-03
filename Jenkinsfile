pipeline {
    agent { label "jenkins-slave" }

    stages {
        stage('Checkout') {
            steps {
               git branch: 'main', url: 'https://github.com/imkiran13/Django-Notes-App.git'
            }
        }
        
        stage('Docker build') {
            steps {
               sh 'docker build -t notes-app:latest .'
            }
        }
        
        stage('Push image to Dockerhub') {
            steps {
               withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]) {
                   sh 'docker login -u $dockerHubUser -p $dockerHubPass'
                   sh 'docker tag notes-app:latest $dockerHubUser/notes-app:latest'
                   sh 'docker push $dockerHubUser/notes-app:latest'
               }
            }
        }
        
        stage('Docker Deploy') {
            steps {
            sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}


pipeline {
    agent any
    stages {
        stage('Cleanup') {
            steps {
                cleanWs()
            }
        }

        stage('Clone Git Repo') {
            steps {
                checkout scm
            }
        }

        stage('Pull Docker Image and Deploy') {
            steps {
                echo 'Pulling and Running Docker Image'
                withCredentials([usernamePassword(credentialsId: 'dockerhub-auth', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh '''
                        docker login -u ${USERNAME} -p ${PASSWORD}
                        docker pull 934372/nginx:latest
                        docker run -d -p 8082:80 934372/nginx:latest
                    '''
                }
            }
        }

        stage('Testing') {
            steps {
                echo 'Testing the Deployed Website'
                sh 'curl -I '18.118.48.215:8082' 
            }
        }
    }
}

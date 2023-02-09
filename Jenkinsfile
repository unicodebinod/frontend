pipeline {
    agent {
        node {
            label 'webserver'
        }
    }

    stages {
        stage('Build') {
            steps {
              sh '''
                docker build -t unicodebinod/frontend:jenkins-${BUILD_NUMBER} .
              '''
            }
        }
        stage('Release') {
            steps {
               withCredentials([usernamePassword(credentialsId: 'dockerhubcredentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) { 
                sh '''
                docker login -u $USERNAME -p $PASSWORD
                docker push unicodebinod/frontend:jenkins-${BUILD_NUMBER}
                '''
              }
            }
        }
        stage('deploy') {
            steps {
                sh '''
                docker stop frontend || true
                docker rm -f frontend || true
                docker run -p3000:80 -d --name frontend unicodebinod/frontend:jenkins-${BUILD_NUMBER}
                '''
            }
        }
    }
}
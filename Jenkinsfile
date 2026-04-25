pipeline {
    agent any
    environment {
        IMAGE_NAME      = "myapp"
        DOCKER_HUB_USER = "urk23cs1209"
    }
    stages {
        stage('Clone') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/URK23CS1209/jen.git'
            }
        }
        stage('Build Image') {
            steps {
                sh 'docker build -t $DOCKER_HUB_USER/$IMAGE_NAME:latest .'
            }
        }
        stage('Push to Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                      echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                      docker push $DOCKER_HUB_USER/$IMAGE_NAME:latest
                    '''
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker run -d -p 5000:5000 $DOCKER_HUB_USER/$IMAGE_NAME:latest'
            }
        }
    }
}

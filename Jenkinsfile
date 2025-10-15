pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "bhanu/cicdproj:latest"   // Change if pushing to Docker Hub
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'git@github.com:<username>/CICDproj.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Stop old container if exists
                    sh '''
                    docker rm -f cicdproj || true
                    docker run -d -p 5000:5000 --name cicdproj $DOCKER_IMAGE
                    '''
                }
            }
        }

        stage('Push to Docker Hub (Optional)') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh '''
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push $DOCKER_IMAGE
                    '''
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline finished!"
        }
    }
}


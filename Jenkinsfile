pipeline {
    agent any 

    environment {
        IMAGE_NAME = "adomicarts/nodeapp-cuban:${BUILD_NUMBER}"
    }
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/HGSChandeepa/test-node'
                }
            }
        }
        stage('Build Docker Image') {
            steps {  
                sh 'docker build -t $IMAGE_NAME .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'docker_p455_ID', variable: 'docker_p455')]) {
                    sh '''
                        echo "Loging to docker"
                        echo "$docker_p455" | docker login -u janithdev --password-stdin
                        echo "Done"
                        '''
}
            }
        }
        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE_NAME'
            }
        }
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}

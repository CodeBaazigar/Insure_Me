pipeline {
    agent any

    stages {
        stage(' Git Checkout ') {
            steps {
                git branch: 'main', credentialsId: 'Github_Credentials', url: 'https://github.com/CodeBaazigar/Insure_Me.git'
            }
        }
        stage(' Build Stage ') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Dcoker Build Image ') {
            steps {
                sh 'docker build -t codebaazigar/insureme .'
            }
        }
        stage(' Pushing To Dockerhub ') {
            steps {
                withDockerRegistry(credentialsId: 'Dockerhub_Credentials', url: 'https://index.docker.io/v1/') {
                    sh 'docker push codebaazigar/insureme'
                }    
            }
        }
        stage(' Deploy to EC2 - Prod Server ') {
            steps {
                sshagent(['Node_Config']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@13.51.109.107 "
                    docker pull codebaazigar/insureme
                    docker run -d --name insureme -p 8081:8081 codebaazigar/insureme
                    "
                    '''
                }
            }
        }
    }
}

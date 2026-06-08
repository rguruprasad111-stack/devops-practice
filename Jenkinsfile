pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Deploy to AWS') {
            steps {
                sshagent(credentials: ['aws-ec2-key']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@http://35.169.191.15 "
                    cd ~/devops-practice &&
                    git pull &&
                    docker build -t aws-test-app . &&
                    docker stop aws-app || true &&
                    docker rm aws-app || true &&
                    docker run -d --name aws-app -p 80:80 aws-test-app
                    "
                    '''
                }
            }
        }
    }
}

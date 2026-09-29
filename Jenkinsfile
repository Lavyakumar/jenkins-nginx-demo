pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Lavyakumar/jenkins-nginx-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '/usr/local/bin/docker build -t jenkins-nginx-demo .'
            }
        }

        stage('Test') {
            steps {
                sh '''
                    /usr/local/bin/docker run --rm jenkins-nginx-demo \
                    python -c "import app; print('Application test passed')"
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    /usr/local/bin/docker stop jenkins-nginx-container || true
                    /usr/local/bin/docker rm jenkins-nginx-container || true
                    /usr/local/bin/docker run -d \
                    --name jenkins-nginx-container \
                    -p 5001:5000 \
                    jenkins-nginx-demo
                '''
            }
        }

        stage('Verify') {
            steps {
                sleep 3
                sh 'curl -f http://localhost:5001'
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline completed successfully!'
        }

        failure {
            echo 'CI/CD Pipeline failed!'
        }
    }
}


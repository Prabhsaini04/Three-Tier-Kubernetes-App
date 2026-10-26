```groovy
pipeline {
    agent any

    options {
        timestamps()
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Source code is checked out by Jenkins.'
            }
        }

        stage('Validate') {
            steps {
                sh '''
                    echo "Checking project files..."

                    test -f docker-compose.yml
                    test -f frontend/Dockerfile
                    test -f backend/Dockerfile

                    echo "Required files are present."
                '''
            }
        }

        stage('Build Frontend Image') {
            steps {
                sh '''
                    docker build \
                        -t three-tier-frontend:${BUILD_NUMBER} \
                        ./frontend
                '''
            }
        }

        stage('Build Backend Image') {
            steps {
                sh '''
                    docker build \
                        -t three-tier-backend:${BUILD_NUMBER} \
                        ./backend
                '''
            }
        }

        stage('Verify Images') {
            steps {
                sh '''
                    docker image inspect three-tier-frontend:${BUILD_NUMBER}
                    docker image inspect three-tier-backend:${BUILD_NUMBER}
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check the console output.'
        }
    }
}
```

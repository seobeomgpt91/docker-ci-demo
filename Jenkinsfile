pipeline {
    agent any

    environment {
        IMAGE_NAME = "id87sbs/study"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Check Files') {
            steps {
                sh '''
                    echo "=== workspace ==="
                    pwd
                    ls -la
                    echo "=== gradle wrapper ==="
                    ls -la gradlew || true
                    ls -la gradle/wrapper || true
                    echo "=== dockerfile ==="
                    ls -la Dockerfile || true
                '''
            }
        }

        stage('Build Jar') {
            steps {
                sh '''
                    sed -i 's/\r$//' gradlew || true
                    chmod +x gradlew || true
                    ./gradlew clean build -x test
                    echo "=== build/libs ==="
                    ls -la build/libs
                '''
            }
        }

        stage('Docker Build') {
            steps {
                sh '''
                    docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                '''
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    '''
                }
            }
        }

        stage('Docker Push') {
            steps {
                sh '''
                    docker push ${IMAGE_NAME}:${IMAGE_TAG}
                '''
            }
        }
    }

    post {
        always {
            sh 'docker logout || true'
        }
    }
}

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

        stage('Check Environment') {
            steps {
                sh '''
                    echo "=== PATH ==="
                    echo $PATH

                    echo "=== whoami ==="
                    whoami || true

                    echo "=== check commands ==="
                    which sh || true
                    which sed || true
                    which chmod || true
                    which docker || true
                    which git || true

                    echo "=== versions ==="
                    docker --version || true
                    git --version || true

                    echo "=== workspace ==="
                    pwd
                    ls -la
                '''
            }
        }

        stage('Build Jar') {
            steps {
                sh '''
                    ls -la gradlew || true
                    sed -i 's/\r$//' gradlew || true
                    chmod +x gradlew || true
                    ./gradlew --version || true
                    ./gradlew clean build -x test
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

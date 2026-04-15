pipeline {
    agent any

    environment {
        IMAGE_NAME = "bsseo/study"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Check Commands') {
            steps {
                sh '''
                    echo "===== PATH ====="
                    echo $PATH

                    echo "===== WHOAMI ====="
                    whoami || true

                    echo "===== GIT ====="
                    which git || true
                    git --version || true

                    echo "===== DOCKER ====="
                    which docker || true
                    docker --version || true

                    echo "===== WORKSPACE ====="
                    pwd
                    ls -la
                '''
            }
        }

        stage('Check Gradle Wrapper') {
            steps {
                sh '''
                    ls -la gradlew || true
                    sed -i 's/\r$//' gradlew || true
                    chmod +x gradlew || true
                    ./gradlew --version || true
                '''
            }
        }
    }
}

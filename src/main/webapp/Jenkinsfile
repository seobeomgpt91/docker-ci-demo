pipeline {
    agent any

    stages {
        stage('git 가져오기') {
            steps {
                git 'https://github.com/seobeomgpt91/docker-ci-demo.git'
            }
        }

        stage('확인') {
            steps {
                sh 'pwd'
                sh 'ls -al'
            }
        }
    }
}
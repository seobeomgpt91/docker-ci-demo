pipeline {
    agent any

    stages {
        stage('git checkout') {
            steps {
                checkout scmGit(
                    branches: [[name: 'main']],
                    userRemoteConfigs: [[url: 'https://github.com/seobeomgpt91/docker-ci-demo.git']]
                )
            }
        }

        stage('check') {
            steps {
                sh 'pwd'
                sh 'ls -al'
            }
        }
    }
}
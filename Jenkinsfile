pipeline {
    agent any

    stages {
        stage('git 가져오기') {
            steps {
                git 'https://github.com/octocat/Hello-World.git'
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
pipeline {
    agent any
    stages {
        // stage('Source') {
        //     steps {
        //         git 'https://github.com/srayuso/unir-cicd.git'
        //     }
        // }
        stage('Build') {
            steps {
                // sh 'ls'
                echo 'Building stage!'
                // sh 'docker --version'
                // sh 'py --version'
                sh 'make build'
            }
        }
        stage('Unit tests') {
            steps {
                sh 'make test-unit'
                archiveArtifacts artifacts: 'results/*.xml'
            }
        }
    }
    post {
        always {
            junit 'results/*_result.xml'
            cleanWs()
        }
    }
}

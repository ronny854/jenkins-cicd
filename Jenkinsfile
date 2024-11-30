pipeline {
    agent any
    stages {
        stage('Source') {
            steps {
                echo 'content form fork repo git'
                sh 'ls'
            }
        }
        stage('Build') {
            steps {
                echo 'Building stage!'
                sh 'make build'
            }
        }
        stage('Unit tests') {
            steps {
                sh 'make test-unit'
                archiveArtifacts artifacts: 'results/*.xml'
            }
        }
        stage('API tests') {
            steps {
                echo 'Running API tests'
                sh 'make test-api'
                archiveArtifacts artifacts: 'results/api/*.xml'
            }
        }
        stage('E2E tests') {
            steps {
                echo 'Running E2E tests'
                sh 'make test-e2e'
                archiveArtifacts artifacts: 'results/e2e/*.xml'
            }
        }
    }
    post {
        always {
            junit 'results/*_result.xml'
            cleanWs()
        }
        failure {
            script {
                def jobName = env.JOB_NAME ?: 'Unknown Job'
                def buildNumber = env.BUILD_NUMBER ?: 'Unknown Build'
                echo "Sending email: The job '${jobName}' (#${buildNumber}) has failed."
                // mail to: 'team@example.com',
                //      subject: "Job '${jobName}' (#${buildNumber}) Failed",
                //      body: "The pipeline job '${jobName}' has failed in build #${buildNumber}."
            }
        }
    }
}

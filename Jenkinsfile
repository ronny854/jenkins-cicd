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
                archiveArtifacts artifacts: 'results/*.xml'
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
                // Obtener el nombre del stage fallido
                def failedStage = env.STAGE_NAME ?: 'Unknown Stage'
                
                def jobName = env.JOB_NAME ?: 'Unknown Job'
                def buildNumber = env.BUILD_NUMBER ?: 'Unknown Build'
                echo "Sending email: The stage '${failedStage}' in job '${jobName}' (#${buildNumber}) has failed."
                
                // Descomenta la línea de envío de correo si es necesario
                // mail to: 'team@example.com',
                //      subject: "Job '${jobName}' (#${buildNumber}) Failed",
                //      body: "The stage '${failedStage}' in job '${jobName}' has failed in build #${buildNumber}."
            }
        }
    }
}

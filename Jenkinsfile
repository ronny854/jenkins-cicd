pipeline {
    agent any

    environment {
        failedStage = ''
    }
    stages {
        stage('Source') {
            steps {
                script{
                    env.failedStage = 'Source'
                    echo 'content form fork repo git'
                    ls
                }
                // sh 'ls'
            }
        }
        stage('Build') {
            steps {
                script{
                    env.failedStage = 'Build'
                    echo 'Building stage!'
                    make build
                }
                // sh 'make build'
            }
        }
        stage('Unit tests') {
            steps {
                script{
                    env.failedStage = 'Unit tests'
                    make test-unit
                }
                archiveArtifacts artifacts: 'results/*.xml'
            }
        }
        stage('API tests') {
            steps {
                script{
                    env.failedStage = 'API tests'
                    echo 'Running API tests'
                    make test-api                   
                }
                // sh 'make test-api'
                archiveArtifacts artifacts: 'results/api/*.xml'
            }
        }
        stage('E2E tests') {
            steps {
                script{
                    env.failedStage = 'E2E tests'
                    echo 'Running E2E tests'
                    make test-e2e
                }
                // sh 'make test-e2e'
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
                def failedStageName = env.failedStage ?: 'Unknown Stage'
                
                def jobName = env.JOB_NAME ?: 'Unknown Job'
                def buildNumber = env.BUILD_NUMBER ?: 'Unknown Build'
                echo "Sending email: The stage '${failedStageName}' in job '${jobName}' (#${buildNumber}) has failed."
                
                // Descomenta la línea de envío de correo si es necesario
                // mail to: 'team@example.com',
                //      subject: "Job '${jobName}' (#${buildNumber}) Failed",
                //      body: "The stage '${failedStage}' in job '${jobName}' has failed in build #${buildNumber}."
            }
        }
    }
}

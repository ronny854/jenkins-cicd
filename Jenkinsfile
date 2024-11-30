
def FAILE_STAGE
pipeline {
    agent any
    stages {
        stage('Source') {
            steps {
                script{
                    FAILE_STAGE = 'Source'
                    echo 'content form fork repo git'
                }
                sh 'ls'
            }
        }
        stage('Build') {
            steps {
                script{
                    FAILE_STAGE = 'Build'
                    echo 'Building stage!'
                }
                sh 'make build'
            }
        }
        stage('Unit tests') {
            steps {
                script{
                    FAILE_STAGE = 'Unit tests'
                }
                sh 'make test-unit'
                archiveArtifacts artifacts: 'results/*.xml'
            }
        }
        stage('API tests') {
            steps {
                script{
                    FAILE_STAGE = 'API tests'
                    echo 'Running API tests'               
                }
                sh 'make test-api'
                archiveArtifacts artifacts: 'results/*.xml'
            }
        }
        stage('E2E tests') {
            steps {
                script{
                    FAILE_STAGE = 'E2E tests'
                    echo 'Running E2E tests'
                }
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
                def jobName = env.JOB_NAME ?: 'Unknown Job'
                def buildNumber = env.BUILD_NUMBER ?: 'Unknown Build'
                echo "Sending email: The stage '${FAILE_STAGE}' in job '${jobName}' (#${buildNumber}) has failed."
                // mail to: 'team@example.com',
                //      subject: "Job '${jobName}' (#${buildNumber}) Failed",
                //      body: "The stage '${FAILE_STAGE}' in job '${jobName}' has failed in build #${buildNumber}."
            }
        }
    }
}

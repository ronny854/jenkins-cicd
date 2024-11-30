pipeline {
    agent any
    environment {
        failedStage = ''
    }
    stages {
        stage('Source') {
            steps {
                script {
                    try {
                        git 'https://github.com/srayuso/unir-cicd.git'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        env.failedStage = 'Source'
                        throw e
                    }
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    try {
                        echo 'Building stage!'
                        sh 'make build'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        env.failedStage = 'Build'
                        throw e
                    }
                }
            }
        }
        stage('Unit tests') {
            steps {
                script {
                    try {
                        echo 'Running unit tests'
                        sh 'make test-unit'
                        archiveArtifacts artifacts: 'results/unit/*.xml'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        env.failedStage = 'Unit tests'
                        throw e
                    }
                }
            }
        }
        stage('API tests') {
            steps {
                script {
                    try {
                        echo 'Running API tests'
                        sh 'make test-api'
                        archiveArtifacts artifacts: 'results/api/*.xml'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        env.failedStage = 'API tests'
                        throw e
                    }
                }
            }
        }
        stage('E2E tests') {
            steps {
                script {
                    try {
                        echo 'Running E2E tests'
                        sh 'make test-e2e'
                        archiveArtifacts artifacts: 'results/e2e/*.xml'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        env.failedStage = 'E2E tests'
                        throw e
                    }
                }
            }
        }
    }
    post {
        always {
            echo 'Archiving test results and cleaning workspace'
            junit 'results/**/*.xml'
            cleanWs()
        }
        failure {
            script {
                // Si alguna etapa falló, se recoge el nombre del stage
                def jobName = env.JOB_NAME ?: 'Unknown Job'
                def buildNumber = env.BUILD_NUMBER ?: 'Unknown Build'
                def failedStage = env.failedStage ?: 'Unknown Stage'
                
                echo "Sending email: The stage '${failedStage}' in job '${jobName}' (#${buildNumber}) has failed."
            }
        }
    }
}

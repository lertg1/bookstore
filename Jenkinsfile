pipeline {
    agent any

    environment {
        // Define any global environment variables here
        GIT_REPO = 'https://github.com/lertg1/bookstore.git'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: "${GIT_REPO}", branch: 'master'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
                dir('api') {
                    sh 'npm install'
                }
            }
        }
        stage('Lint') {
            steps {
                sh 'npm run lint'
                dir('api') {
                    sh 'npm run lint'
                }
            }
        }
        stage('Build') {
            steps {
                sh 'npm run build'
                dir('api') {
                    sh 'npm run build'
                }
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
                dir('api') {
                    sh 'npm test'
                }
            }
        }
        stage('Archive Results') {
            steps {
                junit 'test-results/**/*.xml'
                archiveArtifacts artifacts: 'build/**', allowEmptyArchive: true
                dir('api') {
                    junit 'test-results/**/*.xml'
                    archiveArtifacts artifacts: 'build/**', allowEmptyArchive: true
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}

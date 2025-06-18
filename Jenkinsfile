pipeline {
    agent any
        tools {
        nodejs 'NodeJS'  // Must match the name in Global Tool Configuration
    }

    environment {
        // Define any global environment variables here
        GIT_REPO = 'https://github.com/lertg1/bookstore.git'
    }

    stages {
       stage('Checkout Code') {
            steps {
                checkout([  // Explicit checkout
                    $class: 'GitSCM',
                    branches: [[name: '*/master']],
                    extensions: [],
                    userRemoteConfigs: [[url: 'https://github.com/lertg1/bookstore.git']]
                ])
            }
        }
        
        stage('Install Dependencies') {
            steps {
                script {
                    // Verify files were checked out
                    sh 'ls -la'
                    
                    // If package.json is in a subfolder (e.g. 'frontend')
                    dir('frontend') {
                        sh 'npm install'
                    }
                }
            }
        }
                stage('Checkout') {
            steps {
                checkout scm  // Pulls code from Git
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'  // Now package.json should exist
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

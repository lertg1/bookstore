pipeline {
    agent any
        tools {
        nodejs 'NodeJS'  // Must match the name in Global Tool Configuration
    }

    environment {
        // Define any global environment variables here
        GIT_REPO = 'https://github.com/your-org/your-bookstore-repo.git'
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
                sh 'npm install'
                dir('api') {
                    sh 'npm install'
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
                script {
                    if (fileExists('package.json')) {
                        sh 'npm run lint || true'
                    }
                    if (fileExists('api/package.json')) {
                        dir('api') {
                            sh 'npm run lint || true'
                        }
                    }
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    if (fileExists('package.json')) {
                        sh 'npm run build'
                    }
                    if (fileExists('api/package.json')) {
                        dir('api') {
                            sh 'npm run build'
                        }
                    }
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    if (fileExists('package.json')) {
                        sh 'npm test || true'
                    }
                    if (fileExists('api/package.json')) {
                        dir('api') {
                            sh 'npm test || true'
                        }
                    }
                }
            }
        }
        stage('Archive Results') {
            steps {
                script {
                    if (fileExists('test-results')) {
                        junit 'test-results/**/*.xml'
                    }
                    if (fileExists('build')) {
                        archiveArtifacts artifacts: 'build/**', allowEmptyArchive: true
                    }
                    if (fileExists('api/test-results')) {
                        dir('api') {
                            junit 'test-results/**/*.xml'
                        }
                    }
                    if (fileExists('api/build')) {
                        dir('api') {
                            archiveArtifacts artifacts: 'build/**', allowEmptyArchive: true
                        }
                    }
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

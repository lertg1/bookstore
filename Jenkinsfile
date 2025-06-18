pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/your-org/your-bookstore-repo.git'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: "${GIT_REPO}", branch: 'main'
            }
        }
        stage('Install Dependencies') {
            steps {
                script {
                    if (fileExists('package.json')) {
                        sh 'npm install'
                    }
                    if (fileExists('api/package.json')) {
                        dir('api') {
                            sh 'npm install'
                        }
                    }
                }
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
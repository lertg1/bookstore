pipeline {
    agent any
    
    tools { 
		        // Specify the Maven tool to use
        maven 'M3'
    }


    stages {
        stage('Checkout') {
            steps {
                // Clone the repository from GitHub
                git branch: 'master', url: 'https://github.com/lertg1/bookstore.git'
            }
        }

        stage('Build') {
            steps {
                // Build the application using Maven
                sh 'mvn clean package'
                sh 'mvn clean package -X'
            }
        }

        stage('Test') {
            steps {
                // Run tests using Maven
                sh 'mvn test'
            }
        }

        stage('Archive Results') {
            steps {
                // Archive test results
                junit '**/target/surefire-reports/*.xml'
            }
        }
    }

    post {
        always {
            // Clean up workspace after the build
            cleanWs()
        }
        success {
            echo 'Build and tests completed successfully!'
        }
        failure {
            echo 'Build or tests failed.'
        }
    }
}
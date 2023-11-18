pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout your source code repository (e.g., Git)
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Set up Maven
                tools {
                    maven 'Maven-3.6.3' // Specify the Maven installation name
                }

                // Build the Maven project
                script {
                    def mvnHome = tool 'Maven-3.8.1'
                    def mvnCmd = "${mvnHome}/bin/mvn"
                    sh "${mvnCmd} clean install"
                }
            }
        }

        stage('Test') {
            steps {
                // Run tests using Maven
                script {
                    def mvnHome = tool 'Maven-3.8.1'
                    def mvnCmd = "${mvnHome}/bin/mvn"
                    sh "${mvnCmd} test"
                }
            }
        }

        stage('Deploy') {
            steps {
                // Add deployment steps if needed
            }
        }
    }

    post {
        success {
            // Add post-build actions if needed
        }

        failure {
            // Add actions to be taken on build failure
        }
    }
}

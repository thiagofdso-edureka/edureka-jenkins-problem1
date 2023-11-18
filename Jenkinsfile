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

    }

    post {
        success {
            echo "Build success"
        }

        failure {
            echo "Build failed"
        }
    }
}

pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout your source code repository (e.g., Git)
                checkout scm
            }
        }


        stage('Test') {
            steps {
                // Run tests using Maven
                script {
                    def mvnHome = tool 'Maven-3.6.3'
                    def mvnCmd = "${mvnHome}/bin/mvn"
                    sh "${mvnCmd} test"
                }
            }
        }
	    stage('Parallel Package') {
           parallel {
                stage('Development') {
                    steps {
                        script {
                            echo "Building for Development environment"
                            def mvnHome = tool 'Maven-3.6.3'
                            def mvnCmd = "${mvnHome}/bin/mvn"
                 		     sh "${mvnCmd} clean package -P development -DskipTests=true"
                        }
                    }
                }

                stage('Production') {
                    steps {
                        script {
                            echo "Building for Production environment"
                            def mvnHome = tool 'Maven-3.6.3'
                            def mvnCmd = "${mvnHome}/bin/mvn"
                 		     sh "${mvnCmd} clean package -P production -DskipTests=true"
                        }
                    }
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

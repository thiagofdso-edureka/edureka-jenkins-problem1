pipeline {
    agent  {
    	label "ubuntu"
     }


    environment {
            EMAILS = "thiagofdso.ufpa@gmail.com"
    }

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

	    stage('Parallel Build') {
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
                echo "Sending success mail"
                emailext body: '${TEMPLATE, file="managed:SuccessMail-Body"}', subject: '${TEMPLATE, file="managed:SuccessMail-Title"}', to: "${EMAILS}"
		def mensagemSlack = "${PROJECT_NAME} - ${BUILD_NUMBER} Build Success - ${BUILD_URL}/console"
		slackSend color: "good", message: mensagemSlack
        }
            
        unsuccessful {
                echo "Sending failed mail"
                emailext attachLog: true, body: '${TEMPLATE, file="managed:FailedMail-Body"}', subject: '${TEMPLATE, file="managed:FailedMail-Title"}', to: "${EMAILS} "
                slackSend color: "good", message: "${PROJECT_NAME} - ${BUILD_NUMBER} Build Failed - ${BUILD_URL}/console"
        }
    }
}

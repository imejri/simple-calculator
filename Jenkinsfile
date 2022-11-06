pipeline {
    agent { node { label 'maven' } }
    options { 
        // dont execute the declarative default checkout
        skipDefaultCheckout true 
    }

    stages {
        stage('Checkout') {
            steps {
                sendNotification('started', 'team@example.com')
                git branch: 'notifications',
                url: 'https://github.com/imejri/simple-calculator.git'
            } 
            echo "the name of the stage is ${STAGE_NAME}"
        }

        stage('Test') {
            steps {
               sh './mvnw clean test'
            }
        } 

        stage('Check Style') {
            steps {
                script { 
                    try {
                        sh './mvnw clean checkstyle:check'
                    } //try
                    catch (Exception e) {
                    sendWarningAlert("${STAGE_NAME}", 'devs@example.com')
                    unstable("${STAGE_NAME} stage failed!")
                    } // catch
                }
            }
        }
    }
        

    post {
        failure {
            sendFailureAlert('team@example.com')
        }
        always {
            sendNotification('finished', 'team@example.com')
        }
    }
}
void sendNotification(String action, String email) {
    mail to: "${email}",
    subject: "Pipeline ${action}: ${currentBuild.fullDisplayName}",
    body: "The following pipeline ${action}: ${env.BUILD_URL}"
}
void sendFailureAlert(String email) {
mail to: "${email}",
subject: "Pipeline failed: ${currentBuild.fullDisplayName}",
body: "The following pipeline failed: ${env.BUILD_URL}"
}
void sendWarningAlert(String stage, String email) {
    mail to: "${email}",
}
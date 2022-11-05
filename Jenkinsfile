pipeline {
    agent { node { label 'maven' } }
    options { 
        skipDefaultCheckout true 
    }

    stages {
        stage('Checkout') {
            steps {
                sendNotification('started', 'team@example.com')
                git branch: 'notifications',
                url: 'https://github.com/imejri/simple-calculator.git'
            } 
        }

        stage('Test') {
            steps {
               sh './mvnw clean test'
            }
        } 
        stage('Check Style') {
            steps {
                sh './mvnw clean checkstyle:check'
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
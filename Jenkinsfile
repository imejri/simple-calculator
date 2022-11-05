pipeline {
    agent { node { label 'maven' } }
    options { 
        skipDefaultCheckout true 
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'notifications',
                url: 'https://github.com/imejri/simple-calculator.git'
            } 
        }

        stage('Test') {
            steps {
               sh './mvnw clean test'
            }
        } 
    }
}
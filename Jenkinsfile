pipeline {
    agent any
    environment {
        // Ajouter la variable DH_CRED comme variables d'authentification
        DH_CRED = credentials('dh_cred')
    }
    triggers {
        pollSCM('*/5 * * * *') // Vérifier toutes les 5 minutes
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Init') {
            steps {
                // Permet l'authentification
                script {
                    withCredentials([usernamePassword(credentialsId: 'dh_cred', usernameVariable: 'DH_USERNAME', passwordVariable: 'DH_PASSWORD')]) {
                        sh "docker login -u \$DH_USERNAME -p \$DH_PASSWORD"
                    }
                }
            }
        }
        stage('Build') {
            steps {
                sh 'docker build -t saffar29/express_sqlite:\$BUILD_ID .'
            }
        }
        stage('Deliver') {
            steps {
                sh 'docker push saffar29/express_sqlite:\$BUILD_ID'
            }
        }
        stage('Cleanup') {
            steps {
                sh 'docker rmi saffar29/express_sqlite:\$BUILD_ID'
                sh 'docker logout'
            }
        }
    }
}

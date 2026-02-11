pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                echo 'Récuperation du code ...'
                git branch : 'master',
                url : "https://github.com/Linspi/Projet-DevOps.git"
            }
        }
        stage('Vérification de maven'){
            steps {
                echo ('Vérification de maven')
                sh 'mvn --version'
            }
        }
    }
}

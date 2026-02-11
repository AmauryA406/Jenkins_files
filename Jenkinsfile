pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                echo 'Récuperation du code ...'
                git branch : 'main',
                url : "https://github.com/AmauryA406/Jenkins_files"
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

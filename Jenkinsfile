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
	        stage('MVN COMPILE') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('MVN SONARQUBE') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
 	stage('MVN package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }

    }
}

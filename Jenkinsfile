pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "rimo92/student-management"
        DOCKER_TAG = "latest"
        K8S_NAMESPACE = "devops"
    }

    stages {

        stage('Git Checkout') {
            steps {
                echo 'Récupération du code...'
                git branch: 'main',
                    url: 'https://github.com/AmauryA406/Jenkins_files'
            }
        }

        stage('Vérification de Maven') {
            steps {
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

        stage('MVN Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }

        stage('Docker Build & Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh """
                        docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} .
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push ${DOCKER_IMAGE}:${DOCKER_TAG}
                    """
                }
            }
        }

        stage('Kubernetes Deploy') {
            steps {
                sh """
                    kubectl create namespace ${K8S_NAMESPACE} --dry-run=client -o yaml | kubectl apply -f -
                    kubectl apply -f k8s/pv-sql.yaml
                    kubectl apply -f k8s/pvc-sql.yaml
                    kubectl apply -f k8s/deploy-sql.yaml
                    kubectl apply -f k8s/service-sql.yaml
                    kubectl apply -f k8s/configmap-spring.yaml
                    kubectl apply -f k8s/secret-spring.yaml
                    kubectl apply -f k8s/deploy-spring.yaml
                    kubectl apply -f k8s/service-spring.yaml
                """
            }
        }

        stage('Vérification déploiement') {
            steps {
                sh """
                    kubectl get pods -n ${K8S_NAMESPACE}
                    kubectl get services -n ${K8S_NAMESPACE}
                """
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline terminé avec succès !'
        }
        failure {
            echo '❌ Pipeline échoué.'
        }
    }
}

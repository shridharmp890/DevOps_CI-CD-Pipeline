pipeline {
    agent any

    environment {
        IMAGE_NAME   = "shridhar8899/flask-devops-app"
        DOCKER_HOST  = "tcp://localhost:2375"

        PROJECT_ID   = "devops-k8s-project-497606"
        CLUSTER_NAME = "flask-cluster111"
        CLUSTER_ZONE = "us-central1-a"

        DEPLOYMENT_NAME = "flask-devops-app"
        CONTAINER_NAME  = "flask-devops-app"

        USE_GKE_GCLOUD_AUTH_PLUGIN = "True"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/shridharmp890/DevOps_CI-CD-Pipeline.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %IMAGE_NAME%:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    bat '''
                        docker login -u %DOCKER_USER% -p %DOCKER_PASS%

                        docker push %IMAGE_NAME%:latest
                    '''
                }
            }
        }

        stage('Deploy to GKE') {
            steps {

                withCredentials([file(
                    credentialsId: '110858785515870588284',
                    variable: 'GCP_KEY'
                )]) {

                    bat '''
                        gcloud auth activate-service-account --key-file=%GCP_KEY%

                        gcloud config set project %PROJECT_ID%

                        gcloud container clusters get-credentials %CLUSTER_NAME% --zone %CLUSTER_ZONE% --project %PROJECT_ID%

                        kubectl apply -f k8s/deployment.yaml --validate=false

                        kubectl apply -f k8s/service.yaml --validate=false

                        kubectl set image deployment/%DEPLOYMENT_NAME% %CONTAINER_NAME%=%IMAGE_NAME%:latest

                        kubectl rollout restart deployment/%DEPLOYMENT_NAME%

                        kubectl rollout status deployment/%DEPLOYMENT_NAME%
                    '''
                }
            }
        }
    }

    post {

        success {
            echo 'Deployed to GKE successfully! 🚀'
        }

        failure {
            echo 'Pipeline failed. Check logs.'
        }

        always {
            bat 'docker logout'
        }
    }
}

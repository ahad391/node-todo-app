pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    def app = 'my-todo-app'
                    def version = env.BUILD_NUMBER
                    def image = "${app}:${version}"
                    sh "docker build -t ${image} ."
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    def app = 'my-app'
                    def version = env.BUILD_NUMBER
                    def image = "${app}:${version}"
                    withCredentials([usernamePassword(credentialsId: 'DockerHub', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USERNAME')]) {
                        sh "docker login -u ${DOCKER_HUB_USERNAME} -p ${DOCKER_HUB_PASSWORD}"
                        sh "docker push ${image}"
                    }
                }
            }
        }
        stage('Deploy to GKE') {
            steps {
                script {
                    withCredentials([file(credentialsId: 'gke_credentials', variable: 'GKE_CREDENTIALS')]) {
                        sh 'gcloud auth activate-service-account --key-file=$GKE_CREDENTIALS'
                        sh 'gcloud container clusters get-credentials my-cluster --zone us-central1-a --project my-project'
                    }
                    def app = 'my-app'
                    def version = env.BUILD_NUMBER
                    def image = "${app}:${version}"
                    sh "kubectl set image deployment/${app} ${app}=${image}"
                }
            }
        }
    }
}

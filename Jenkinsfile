pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    def app = 'my-todo-app'
                    def version = env.BUILD_NUMBER
                    def image = "${app}:${version}"
                    sh "docker build -t ahad391/${image} ."
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    def app = 'my-todo-app'
                    def version = env.BUILD_NUMBER
                    def image = "${app}:${version}"
                    withCredentials([usernamePassword(credentialsId: 'DockerHub', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USERNAME')]) {
                        sh "docker login -u ${DOCKER_HUB_USERNAME} -p ${DOCKER_HUB_PASSWORD}"
                        sh "docker push ${DOCKER_HUB_USERNAME}/${image}"
                    }
                }
            }
        }

        stage('Deploy to GKE') {
            steps {
                script {
                    
                    sh "sudo kubectl apply -f deployment.yaml"
                }
            }
        }
        
    }
}

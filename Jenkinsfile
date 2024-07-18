pipeline {
    agent any
    
    environment {
        DOCKER_REGISTRY = 'docker.io'
        DOCKER_CREDENTIALS_ID = 'dockerHubCredentials'
    }

    stages {
        stage('Checkout') {
            steps {
                git url:'https://github.com/SatishAero/Task_3_18_july_2024.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def tagName = "v1"
                    def imageName = "satishvarma123/performance:${tagName}"
                    
                    docker.build(imageName, "-f Dockerfile .")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    def tagName = "v1"
                    def imageName = "satishvarma123/performance:${tagName}"
                    docker.withRegistry("https://index.docker.io/v1/", DOCKER_CREDENTIALS_ID) {
                        docker.image(imageName).push()
                    }
                }
            }
        }


        stage('Run Docker Container') {
            steps {
                script {
                    def tagName = "v1"
                    def containerName = "performance-${tagName}"
                    def imageName = "satishvarma123/performance:${tagName}"
                     bat "docker rm -f ${containerName} || true"
                    bat "docker run -d -p 8051:80 --name ${containerName} ${imageName}"
                }
            }
        }

       
    }

    post {
        always {
            cleanWs()
        }
    }
}

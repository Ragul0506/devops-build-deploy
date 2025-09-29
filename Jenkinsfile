pipeline {
    agent any

    environment {
        DEV_REPO = "sarwanragul/sarwanragul-devops-build-dev"
        PROD_REPO = "sarwanragul/sarwanragul-devops-build-prod"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: "${env.BRANCH_NAME}", url: 'https://github.com/Ragul0506/devops-build-deploy.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("myapp:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        if (env.BRANCH_NAME == "dev") {
                            docker.image("myapp:${env.BUILD_NUMBER}").push("latest")
                            docker.image("myapp:${env.BUILD_NUMBER}").push("dev")
                        } else if (env.BRANCH_NAME == "master") {
                            docker.image("myapp:${env.BUILD_NUMBER}").push("latest")
                            docker.image("myapp:${env.BUILD_NUMBER}").push("prod")
                        }
                    }
                }
            }
        }
    }
}


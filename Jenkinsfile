pipeline {
    agent any

    environment {
        DEV_REPO = "sarwanragul/sarwanragul-devops-build-dev"
        PROD_REPO = "sarwanragul/sarwanragul-devops-build-prod"
        IMAGE_NAME = "myapp"
        DOCKER_CREDENTIALS_ID = "dockerhub-credentials"
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    // Default to 'dev' branch if BRANCH_NAME not set
                    def branch = env.BRANCH_NAME ?: 'dev'
                    echo "Checking out branch: ${branch}"
                    checkout([$class: 'GitSCM',
                        branches: [[name: branch]],
                        userRemoteConfigs: [[url: 'https://github.com/Ragul0506/devops-build-deploy.git']]
                    ])
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image using React build folder
                    docker.build("${IMAGE_NAME}", ".")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        def branch = env.BRANCH_NAME ?: 'dev'

                        if (branch == "dev") {
                            sh "docker tag("${IMAGE_NAME}") $DEV_REPO"
                            docker.push("$DEV_REPO")
                            
                        } else if (branch == "master") {
                            docker.image("${IMAGE_NAME}:${env.BUILD_NUMBER}").push("latest")
                            docker.image("${IMAGE_NAME}:${env.BUILD_NUMBER}").push("prod")
                        }
                    }
                }
            }
        }
    }
}



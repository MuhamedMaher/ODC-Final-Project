pipeline {
    agent any  // Use any available agent

    stages {
        stage("Checkout Code") {
            steps {
                script {
                    if (!fileExists('nodejs.org')) {
                        sh 'git clone https://github.com/MuhamedMaher/nodejs.org.git'
                    }
                    dir('nodejs.org') {
                        sh 'git fetch origin'
                        sh 'git checkout main'
                        sh 'git pull'
                    }
                }
            }
        }

        stage("Install Dependencies") {
            steps {
                // Install dependencies using npm
                script {
                    sh 'npm install'
                }
            }
        }

        stage("Run Unit Testing") {
            steps {
                // Run unit tests using npm
                script {
                    sh 'npm test'
                }
            }
        }

        stage("Dockerize") {
            steps {
                // Build the Docker image
                script {
                    sh 'docker build -t mohamedmaher77/nodejs-app .'
                }
            }
        }

        stage("Push Docker Image") {
            steps {
                // Log in to Docker Hub
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    }
                }

                // Push the Docker image to Docker Hub
                script {
                    sh 'docker push mohamedmaher77/nodejs-app'
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker images and containers (optional)
            script {
                sh 'docker rmi mohamedmaher77/nodejs-app || true' // Remove local image, ignore errors if it doesn't exist
            }
        }
    }
}


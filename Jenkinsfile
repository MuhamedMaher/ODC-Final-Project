pipeline {
    agent any 

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

                script {
                    sh 'npm install'
                }
            }
        }

        stage("Run Unit Testing") {
            steps {
        
                script {
                    sh 'npm test'
                }
            }
        }

        stage("Dockerize") {
            steps {
          
                script {
                    sh 'docker build -t mohamedmaher77/nodejs-app .'
                }
            }
        }

        stage("Push Docker Image") {
            steps {
       
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    }
                }

          
                script {
                    sh 'docker push mohamedmaher77/nodejs-app'
                }
            }
        }
    }

    post {
        always {
       
            script {
                sh 'docker rmi mohamedmaher77/nodejs-app || true' 
            }
        }
    }
}


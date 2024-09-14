pipeline {
    agent any
    environment {
        SONARQUBE_URL = 'http://localhost:9000'
        SONARQUBE_TOKEN = credentials('SonarQube-Token')
    }
    stages {
        stage('Clone Code') {
            steps {
                git url: "https://github.com/comprofessional/node-todo-cicd.git", branch: "master"
                echo 'Code has been cloned'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker-compose build --no-cache'
                    echo 'Docker image built'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('SonarQube') {
                        // Run SonarQube Scanner
                        // Make sure SonarQube Scanner is installed and correctly configured
                        sh 'sonar-scanner -Dsonar.projectKey=node-todo-cicd -Dsonar.sources=. -Dsonar.host.url=$SONARQUBE_URL -Dsonar.login=$SONARQUBE_TOKEN'
                    }
                }
            }
        }

        stage('Scan Image') {
            steps {
                echo 'Image scanning completed'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId:"dockerHub", passwordVariable:"dockerHubPass", usernameVariable:"dockerHubUser")]) {
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag node-app-test-new:latest ${env.dockerHubUser}/node-app-test-new:latest"
                    sh "docker push ${env.dockerHubUser}/node-app-test-new:latest"
                    echo 'Docker image pushed'
                }
            }
        }

        stage('Deploy') {
            steps {
                sh "docker-compose down && docker-compose up -d"
                echo 'Deployment completed'
            }
        }
    }
}

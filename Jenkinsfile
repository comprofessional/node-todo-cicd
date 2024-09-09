pipeline {
    agent any // { label "dev-server"}
    
    stages {
        
        stage("code"){
            steps{
                git url: "https://github.com/comprofessional/node-todo-cicd.git", branch: "master"
                echo 'bhaiyya code clone ho gaya'
            }
        }
        //stage("build and test"){
           // steps{
             //   sh "docker build -t node-app-test-new ."
               // echo 'code build bhi ho gaya'
          //  }
        // }
        stage('Build Docker Image') {
               steps {
                 script {
                 sh 'docker-compose build --no-cache'
            }
        }
    }

                
        
        stage("scan image"){
            steps{
                echo 'image scanning ho gayi'
            }
        }
        stage("push"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker tag node-app-test-new:latest ${env.dockerHubUser}/node-app-test-new:latest"
                sh "docker push ${env.dockerHubUser}/node-app-test-new:latest"
                echo 'image push ho gaya'
                }
            }
        }
        stage("deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
                echo 'deployment ho gayi'
            }
        }
    }
}

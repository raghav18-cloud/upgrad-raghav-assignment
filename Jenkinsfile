pipeline {
    agent any
    environment {
        registry = "614217881944.dkr.ecr.us-east-1.amazonaws.com/nodejs-appb274afd63b71"
    }

 stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/raghav18-cloud/upgrad-raghav-assignment.git']]])
            }
        }

stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry
        }
      }
    }

stage('Pushing to ECR') {
     steps{  
         script {
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 614217881944.dkr.ecr.us-east-1.amazonaws.com'
                sh 'docker push 614217881944.dkr.ecr.us-east-1.amazonaws.com/nodejs-appb274afd63b71:latest'
                sh 'sudo su - jenkins_devops'
                def sshCommand = "ssh  app_devops@172.31.84.99 'docker run -d -p 8080:8080 --name nodejs_deploy  614217881944.dkr.ecr.us-east-1.amazonaws.com/nodejs-appb274afd63b71:latest'"
                sh sshCommand
         }
        }
      }

   }

}

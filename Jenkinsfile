pipeline {
    agent any 
    
    tools{
        jdk 'jdk17'
        maven 'maven3'
    }
    
    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }
    
    stages{
        
        stage("Git Checkout"){
            steps{
               git branch: 'main', credentialsId: 'git-cred', url: 'https://github.com/Narayananraj/ansible-automate-deployment.git'
            }
        }
      stage("Compile") {
    steps {
        sh "mvn clean compile"
    }
}

        
         stage("Test Cases"){
            steps{
                sh "mvn test"
            }
        }
        
                     stage("Build"){
            steps{
                sh " mvn clean package"
            }
         }
            

         stage('Docker Build') {
            steps {
               script{
                   withDockerRegistry(credentialsId: 'docker-cred') {
                    sh "docker build -t  ansibledeploy . "
                 }
               }
            }
        }

        stage('Docker Push') {
            steps {
               script{
                   withDockerRegistry(credentialsId: 'docker-cred') {
                    sh "docker tag ansibledeploy narayananraj/ansibledeploy:latest"
                    sh "docker push narayananraj/ansibledeploy:latest"
                 }
               }
            }
        }

        
        stage('Docker Deploy'){
            steps{
             ansiblePlaybook credentialsId: 'dev', disableHostKeyChecking: true, installation: 'ansible', inventory: 'dev.inv', playbook: 'deploy-docker.yml', vaultTmpPath: ''
            }
        }
        
            
            
        }
        }


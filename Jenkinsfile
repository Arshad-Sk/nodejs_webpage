pipeline {
    agent any
    
     tools {
        nodejs 'nodejs-10'
       
    }
    
   
    stages {
      
    
        stage('Git Checkout ') {
            steps {
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/Arshad-Sk/nodejs_webpage.git'
            }
        }
        
          stage('Install Dependencies') {
            steps {
                    sh "npm install"
            }
        }
        
       
         stage("OWASP Dependency Check"){
            steps{
                dependencyCheck additionalArguments: '--scan ./ ', odcInstallation: 'DP-Check'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        
      
        stage("Docker Build & Push"){
            steps{
                script{
                   withDockerRegistry(credentialsId: 'b9f5bd82-bfa9-4e28-a440-6792e37b103e', toolName: 'docker') {
                        
                        sh "docker build -t demonodejs ."
                        sh "docker tag demonodejs sk77arshad/nodejs:latest "
                        sh "docker push sk77arshad/nodejs:latest "
                       
                                           }
                }
            }
        }
        
     
        stage("Deploy using Docker"){
            steps{
                sh "docker run -d --name demonodejs -p 8081:8081 sk77arshad/nodejs:latest"
            }
        }
        
}
}

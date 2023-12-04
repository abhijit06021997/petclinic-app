pipeline {
    agent any 
    
    tools{
        jdk 'jdk17'
        maven 'maven'
    }
    
    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }
    
    stages{
        
        stage("Git Checkout"){
            steps{
                git 'https://github.com/Rahul-ukey/petclinic-app.git'
            }
        }
        
        stage("Compile"){
            steps{
                sh "mvn clean compile"
            }
        }
        
         stage("Test"){
            steps{
                sh "mvn test"
            }
        }
        
        stage("Sonarqube Analysis "){
            steps{
                withSonarQubeEnv('sonar') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Petclinic \
                    -Dsonar.java.binaries=. \
                    -Dsonar.projectKey=Petclinic '''
    
                }
            }
        }
         stage("Build"){
            steps{
                sh " mvn clean install"
            }
        }
        
        stage("Docker Build & Push"){
            steps{
                script{
                   withDockerRegistry(credentialsId: '4ac09102-7c3f-4020-ab55-a50f0005a2d8', toolName: 'docker') {
                        
                        sh "docker build -t petapp ."
                        sh "docker tag petapp rahulukey123/pet-clinicapp:latest "
                        sh "docker push rahulukey123/pet-clinicapp:latest "
                    }
                }
            }
        }
            
        stage("Deploy"){
            steps{
                sh "cp  /var/lib/jenkins/workspace/pipeline/target/petclinic.war /opt/apache-tomcat-8.5.96/webapps/ "
            }
        }
    }
}

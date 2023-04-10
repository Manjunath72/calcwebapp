pipeline {
   
    agent any
    
    tools{
        maven 'maven3.9.1'
    }
    
    stages {
        
        stage('checkout') {
            steps {
                git 'https://github.com/Manjunath72/maven-web-application.git'
            }
        }
        
        stage ('Build') {
            steps {
            sh 'mvn clean package' 
            }
        }
        
        //stage('ExecuteSonarQubeReport'){
            //steps{
            //sh  'mvn sonar:sonar'
            
            //}
        //}
        stage('code review'){
            steps{
                sh 'mvn pmd:pmd'
            }
        }
        
        stage('Nexus Upload'){
            steps{
               nexusArtifactUploader artifacts: [[artifactId: 'maven-web-application', classifier: '', file: 'target/maven-web-application.war', type: 'war']], credentialsId: 'nexuskey', groupId: 'com.mt', nexusUrl: 'http://ec2-65-0-123-209.ap-south-1.compute.amazonaws.com:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-web-application', version: '1.0.1-SNAPSHOT'
            }
        }
        
        stage('DEV deploy'){
            steps{
               deploy adapters: [tomcat8(credentialsId: 'tomcatkey', path: '', url: 'http://ec2-43-205-208-146.ap-south-1.compute.amazonaws.com:8080/')], contextPath: 'maven-web-application', war: '**/*.war'
            }
        }
        
        //stage('Email Notification'){
            //steps{
                //emailext body: 'Deployment was successfull', subject: 'Deployment', to: 'manjunath.h1919@gmail.com'
            //}
        //}
    }
}

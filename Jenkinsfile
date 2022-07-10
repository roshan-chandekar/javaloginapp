pipeline{
    agent any
    environment {
        PATH = "$PATH:/usr/share/maven"
    }
    stages{
       stage('GetCode'){
            steps{
                git 'https://github.com/roshan-chandekar/javaloginapp.git'
            }
         }        
       stage('Build'){
            steps{
                sh 'mvn clean package'
            }
         }
        stage('SonarQube analysis') {
//    def scannerHome = tool 'SonarScanner 4.0';
        steps{
        withSonarQubeEnv('sonarqube-8.9.9') { 
        // If you have configured more than one global server connection, you can specify its name
//      sh "${scannerHome}/bin/sonar-scanner"
        sh "mvn sonar:sonar"
    }
        }
        }
        
        stage('Upload WAR artifacts to NEXUS') {
            
            steps{
                nexusArtifactUploader artifacts: [
                    [
                        artifactId: 'valaxy', 
                        classifier: '', 
                        file: 'target/valaxy-2.0-RELEASE.war', 
                        type: 'war'
                    ]
                ], 
                credentialsId: 'nexus3', 
                groupId: 'in.valaxy', 
                nexusUrl: 'localhost:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'java-application-repo', 
                version: '2.0-RELEASE'
            }
            
        }
       
    }
}

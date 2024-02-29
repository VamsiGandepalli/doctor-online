library("jhc-libs") _

pipeline {
    agent any

    stages {
        stage('Code Checkout') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/VamsiGandepalli/doctor-online'
            }
        }
        stage('Maven Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage("Upload Artifacts") {
            steps{
                nexusArtifactUploader artifacts: [[artifactId: 'doctor-online', classifier: '', file: 'target/doctor-online', type: 'war']], 
                    credentialsId: 'nexus3', 
                    groupId: 'in.javahome', 
                    nexusUrl: '172.31.38.63:8081', 
                    nexusVersion: 'nexus2', 
                    protocol: 'http', 
                    repository: 'do-release', 
                    version: '1.4'
            }
        }
        stage("Tomcat Dev Deploy"){
            steps{
                tomcatDeploy("172.31.12.10","ec2-user","tomcat-dev","doctor-online.war")
                
            }
        }
    }
}

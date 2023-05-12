pipeline {
    agent any
    stages {
        stage ("Git Checkout") {
            steps {
                git branch: 'main', url: 'https://github.com/SukhanthN/Taxi_Booking_Tomcat.git'
            }
        }
        stage ("Maven Build") {
            steps {
                bat "mvn clean install"
            }
        }
        stage ("Code Review") {
            steps {
                withSonarQubeEnv('SonarQube') {
                bat 'mvn sonar:sonar' 
                }   
            }
        }
        stage ("Save Artifact") {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'taxi-booking-1.0.1', classifier: '', file: 'target/taxi-booking-1.0.1.war', type: 'war']], credentialsId: 'nexus', groupId: 'com.example.maven-project', nexusUrl: 'localhost:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'sukhanth', version: '1.0-SNAPSHOT'
            }
        }
        stage ("Deploy") {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://localhost:9090/')], contextPath: null, war: '**/*.war'
            }
        }
    }
}

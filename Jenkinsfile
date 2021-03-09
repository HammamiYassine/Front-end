pipeline{
    agent any
    environment {
        NEXUS_VERSION= "3"
        NEXUS_PROTOCOL="http"
        NEXUS_URL="http://localhost:8082/"
        NEXUS_REPOSITORY="frontend"
        NEXUS_CREDENTIALS_ID="1234"
    }
    stages {
        stage ('SCM'){
            steps{
                git credentialsId: '1a687c74-42ea-427f-bff0-1a63654f3be1',
                url: 'https://github.com/HammamiYassine/Front-end.git'
            }
        }
        stage('Dependencies') {
            steps {
             bat 'npm install'
            }
        }    
         stage('Build') {
            steps {
             bat 'npm run build'
            }
        } 
        stage('zip artifact') {
            steps{
                script{
                    zip archive: true, dir: 'dist', glob: '', zipFile: 'yassine.zip', overwrite: true
                } 
            }
        }
        stage ('pushing artifact to nexus'){
             steps{
               nexusArtifactUploader artifacts: [
                   [
                       artifactId: 'FrontServerSide',
                       classifier: '',
                       file: 'yassine.zip',
                       type: 'zip'
                       ]
                       ],
               credentialsId: '1234',
               groupId: 'fr.Forum',
               nexusUrl: 'localhost:8082/',
               nexusVersion: 'nexus3',
               protocol: 'http',
               repository: 'frontend',
               version: '0.0.1'
             }
        }
        stage ('deploy'){
        steps {
        bat '''docker-compose down
        docker-compose up -d'''
        }
        }
    }
}

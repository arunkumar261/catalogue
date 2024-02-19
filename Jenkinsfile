pipeline {
    agent {
      node{
        label 'AGENT-1'
      }
    }
   environment{
        packageversion = ''
        nexusUrl = '172.31.11.122:8081'
   }
   options{
        timeout(time:1, unit:'HOURS')
        disableConcurrentBuilds()
    }
    //  parameters {
    //     choice(name: 'action', choices: ['apply', 'destroy'], description: 'Pick something')
    // }
    stages {
        stage('Get the version') {
            steps {
               script{
                def packageJson = readJSON file : 'package.json'
                packageversion =packageJson.version
                echo "application version is : $packageversion"
               }
            }
        }
        stage('install dependencies') {
            steps {
              sh """
                    npm install
              """
            }
        }
         
         stage('Build') {
            steps {
                sh """
                zip -q -r catalogue.zip ./* -x ".git" -x "*.zip"
                ls -ltr
                """
            }
        }
        stage('Publsih Artifact') {
            steps {
            nexusArtifactUploader(
                  nexusVersion: 'nexus3',
                  protocol: 'http',
                  nexusUrl: '${nexusUrl}',
                  groupId: 'com.roboshop',
                  version: '${packageversion}',
                  repository: 'catalogue',
                  credentialsId: 'nexus-auth',
                  artifacts: [
                     [artifactId: 'catalogue',
                       classifier: '',
                       file: 'catalogue.zip',
                       type: 'zip']
                    ]
                )
            }
        }
         
        stage('deploy') {
            steps {
                sh """
                    echo "here  im running shell script"
                """
            }
        }
    }
    post { 
        always { 
            echo 'I will always say Hello again!'
            deleteDir()
        }
        failure { 
            echo 'this runs when pipeline is failed, used generally to send some alerts'
        }
        success{
            echo 'I will say Hello when pipeline is success'
        }
    }



}

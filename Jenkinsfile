pipeline {
    agent {
      node{
        label 'AGENT-1'
      }
    }
   environment{
        packageversion = ''
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
         
         stage('Test') {
            steps {
               echo "testing"
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
        }
        failure { 
            echo 'this runs when pipeline is failed, used generally to send some alerts'
        }
        success{
            echo 'I will say Hello when pipeline is success'
        }
    }



}

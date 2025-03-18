pipeline {
    agent {
        node {
            label 'AGENT-1'
        }
    }
    environment {
        packageVersion = ''
    }
    options {
        timeout(time: 1, unit: 'HOURS') //it will allow to run this pipeline for 1 hour to execute
        disableConcurrentBuilds()  //this wont allow to run 2 builds at a time
    }

    //build 
    stages {
        stage('Get the version') {
            steps {
                script{ //script is groovey not shell
                    def packageJson = readJSON file: 'package.json' //def is variable initilization
                    packageVersion = packageJson.version
                    echo "application version: $packageVersion"
                }
            }
        }
        stage('Install dependencies') {
            steps {
                sh """
                    npm install 
                """
            }
        }
        stage('Build') { // added -q to silent the logs otherwise it will consume jenkins master memory
            steps {
                 sh """
                    ls -la
                    zip -q -r catalogue.zip ./* -x ".git" -x ".zip" 
                    ls -ltr
                """
            }
        }
        stage('Deploy') {
            steps {
                sh """
                echo "Here I wrote shell script"
                """
            }
        }

    }
      // post build
        post { 
        always { 
            echo 'I will always say Hello again!'
        }
        failure{
            echo 'this runs when pipeline is failed, used generally to sned alerts'
        }
        success{
            echo 'I will say hello when pipeline is success'
        }
    } // After pipeline directory must be deleted from pipeline using deldir()
}
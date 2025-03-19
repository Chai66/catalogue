pipeline {
    agent {
        node {
            label 'AGENT-1'
        }
    }
    environment {
        packageVersion = ''
        nexusUrl = '172.31.9.236:8081'
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
        stage('Publish Artifact') { 
            steps {
                nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: "${nexusUrl}",
                        groupId: 'com.roboshop',
                        version: "${packageVersion}",
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
        stage('Deploy') { // build job will let catalogue-deploy to wait until catalogue pipeline finsihes
            steps {
                build job: "catalogue-deploy", wait: true, parameters:[
                    string(name: 'version',value: "${packageVersion}")
                    string(name: 'environment',value:'dev')
                ]
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
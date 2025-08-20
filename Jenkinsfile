pipeline {
    agent {
        label 'AGENT-1'
    }

    options {
        timeout(time:30, unit:'MINUTES') // set a timeout of 30 minutes for the entire pipeline
        disableConcurrentBuilds() // disable concurrent builds, only one build can run at a time
    }

    environment {
        appVersion = ''
    }

    stages {
        
        stage('Read package.json') { // stage to read package.json file
            steps {
                script { // using script block to run a Groovy script
                    echo 'Reading package.json...'
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "Application Version: ${appVersion}"
                }
            }
        }

         stage('install dependencies') { // stage to install dependencies
            steps {
                script { // using script block to run a Groovy script
                   sh """
                     npm install

                   """ // using sudo to run npm install command
                    echo 'Dependencies installed successfully.'
                }
            }
        }
    }

}
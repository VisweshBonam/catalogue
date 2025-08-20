pipeline {
    agent {
        label 'AGENT-1'
    }

    options {
        timeout(time: 30, unit: 'MINUTES')   // timeout for pipeline
        disableConcurrentBuilds()            // only one build at a time
    }

    environment {
        appVersion = ''
    }

    stages {

        stage('Cleanup Workspace') {
            steps {
                echo "Cleaning workspace..."
                deleteDir()                  // remove old files to avoid conflicts
            }
        }

        stage('Checkout') {
            steps {
                echo "Checking out source code..."
                retry(3) {                   // retry checkout in case of transient errors
                    checkout scm
                }
            }
        }
        
        stage('Read package.json') {
            steps {
                script {
                    echo 'Reading package.json...'
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "Application Version: ${appVersion}"
                }
            }
        }

        stage('Install dependencies') {
            steps {
                echo "Installing dependencies..."
                sh "npm install"             // no sudo (install locally in workspace)
                echo 'Dependencies installed successfully.'
            }
        }
    }

    post {
        always {
            echo "Cleaning up after build..."
            deleteDir()                      // clean workspace at the end
        }
    }
}

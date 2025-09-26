pipeline {
    agent any
    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm config set cache $(pwd)/.npm-cache
                    npm config set prefix $(pwd)/.npm-prefix
                    npm ci
                '''
            }
        }
    }
}
pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            environment {
                // Use a local cache directory to avoid EACCES errors
                npm_config_cache = "${WORKSPACE}/.npm-cache"
            }
            steps {
                sh '''
                    echo " Listing current directory..."
                    ls -la

                    echo " Checking npm and node versions..."
                    npm --version
                    node --version

                    echo " Creating local npm cache directory..."
                    mkdir -p "$npm_config_cache"

                    echo " Installing dependencies with npm ci..."
                    npm ci
                '''
            }
        }
        stage('Test'){
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                echo 'Test Stage'
                
                sh '''
                    echo " Running tests..."
                    if [ -f src/App.js ]; then
                        echo " src/App.js exists."
                    else
                        echo " src/App.js not found. Skipping test."
                    fi

                    npm test
                '''

            }
        }
        stage('E2E'){
            agent{
                docker {
                    image 'mcr.microsoft.com/playwright:v1.55.0-noble'
                    reuseNode true
                    
                }
            }
            environment {
                // Use a local cache directory to avoid EACCES errors
                npm_config_cache = "${WORKSPACE}/.npm-cache"
            }
            steps{
                sh '''
                    npm install -g serve
                    node_modules/.bin/serve -s build &
                    sleep 5
                    npx playwright test
                '''
            }
        }
    }
    post{
        always{
            junit 'jest-results/junit.xml'
        }
    }
}
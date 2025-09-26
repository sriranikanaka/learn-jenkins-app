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
                    echo "📂 Listing current directory..."
                    ls -la

                    echo "📦 Checking npm and node versions..."
                    npm --version
                    node --version

                    echo "📁 Creating local npm cache directory..."
                    mkdir -p "$npm_config_cache"

                    echo "📦 Installing dependencies with npm ci..."
                    npm ci
                '''
            }
        }
        stage('Test'{
            steps{
                echo 'Test Stage'
                sh '''
                    echo "🧪 Running tests..."
                    test -f 'build/index.html'
                    npm test
                '''
            }
        })
    }
}
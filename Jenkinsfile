pipeline {
    agent any

    stages {
        stage('build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "📁 Creating local npm cache directory..."
                    mkdir -p .npm-cache
                    npm config set cache $(pwd)/.npm-cache --global

                    echo "📦 Installing dependencies..."
                    npm ci

                    echo "🏗️ Building the app..."
                    npm run build

                    echo "📂 Listing build directory..."
                    ls -la build
                '''
            }
        }

        stage('test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "🔍 Checking for index.html in build folder..."
                    if [ -f build/index.html ]; then
                        echo "✅ index.html found."
                    else
                        echo "❌ index.html not found!"
                        exit 1
                    fi

                    echo "🧪 Running tests..."
                    npm test
                '''
            }
        }
    }

    post {
        always {
            echo "📦 Publishing test results (if available)..."
            junit 'test-results/junit.xml'
        }
    }
}
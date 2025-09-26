pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Listing current directory...'
                sh 'ls -la'
                echo 'Checking npm and node versions...'
                sh 'npm --version'
                sh 'node --version'
                echo 'Installing dependencies with npm install...'
                sh 'npm install || { echo "npm install failed, please check package.json and package-lock.json"; exit 1; }'
            }
        }
        stage('Test') {
            steps {
                echo 'Test Stage'
                echo 'Running tests...'
                sh 'if [ -f src/App.js ]; then echo "src/App.js exists."; else echo "src/App.js not found. Skipping test."; fi'
                sh 'npm test'
            }
        }
        stage('E2E') {
    agent {
        docker {
            image 'mcr.microsoft.com/playwright:v1.55.0-noble'
            reuseNode true
        }
    }
    environment {
        npm_config_cache = "${WORKSPACE}/.npm-cache"
    }
    steps {
        sh '''
            echo "📁 Creating local npm cache directory..."
            mkdir -p "$npm_config_cache"

            echo "📦 Installing serve globally..."
            npm install -g serve

            echo "🚀 Starting local server..."
            node_modules/.bin/serve -s build &

            echo "🧪 Running Playwright tests..."
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
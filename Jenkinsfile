pipeline {
    agent any
    environment {
        PATH = "/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:$PATH"
    }
    stages {
        
        stage('Install dependencies') {
            steps {
                sh '''
                    echo "Installing dependencies..."
                    node -v
                    npm ci
                '''
            }
        }

        stage('Run Playwright tests') {
            steps {
                sh '''
                    echo "Running tests..."
                    npx playwright test --reporter=html
                '''
            }
        }

        stage('Prepare HTML Report') {
            steps {
                sh '''
                    echo "Preparing report for Jenkins..."
                    mkdir -p reports
                    cp -r playwright-report/* reports/
                '''
            }
        }

        stage('Publish reports') {
            steps {
                echo 'Publishing HTML test report...'
                publishHTML([
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'reports',
                    reportFiles: 'index.html',
                    reportName: 'Playwright HTML Report',
                    useWrapperFileDirectly: true
                ])
            }
        }
    }
}

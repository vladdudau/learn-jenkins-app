pipeline {

    agent any

    environment {
        NPM_CONFIG_CACHE = "${WORKSPACE}/.npm"
        
    }

    stages {
        
        stage("Build") {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    rm -rf node_modules package-lock.json
                    npm install --prefer-offline
                    npm run build
                '''
            }
        }

        stage("Test") {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Verificam index.html"
                    find build/index.html
                    echo "Rulam testele"
                    npm run test
                '''
            }
        }

         stage("E2E") {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install -g serve
                    node_modules/.bin/serve -s build
                    npx playwright test
                '''
            }
        }
    }

    post{
        always {
            junit 'test-results/junit.xml'
        }
    }
}

pipeline {
    environment {
        NPM_CONFIG_CACHE = "${WORKSPACE}/.npm"
    }

    stages {
         agent {
        docker {
            image 'node:18-alpine'
            args '-m 3g --cpus=2'
        }
    }
        stage("Build") {
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
            args '-m 3g --cpus=2'
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
                    args '-u root:root'
                }
            }
            steps {
                sh '''
                    npm install -g serve
                    serve -s build
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

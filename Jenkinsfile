pipeline {
    agent {
        docker {
            image 'node:18-alpine'
            args '-m 3g --cpus=2'
        }
    }

    environment {
        NPM_CONFIG_CACHE = "${WORKSPACE}/.npm"
    }

    stages {
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
            steps {
                sh '''
                    echo "Verificam index.html"
                    find build/index.html
                    echo "Rulam testele"
                    npm run test
                '''
            }
        }
    }
}

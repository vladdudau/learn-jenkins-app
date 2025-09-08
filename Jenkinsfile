pipeline {
    agent any

    stages {
        stage("Build"){
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    args '--user node -m 2g --cpus=1'
                }
            }
            environment {
                NPM_CONFIG_CACHE = "${WORKSPACE}/.npm" // cache local în workspace
            }
            steps {
                sh '''
                    echo "Workspace: $WORKSPACE"
                    ls -la
                    node --version
                    npm --version
                    rm -rf node_modules package-lock.json
                    npm install --prefer-offline
                    npm run build
                    ls -la
                '''
            }
        }
    }
}

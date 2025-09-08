pipeline {
    agent any

    stages {

        stage("Build"){
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    args '-m 3g --cpus=2'
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

        stage("Test"){
            steps{
                sh '''
                    echo "Stage test"
                    echo "Testam daca avem fisierul index.html"
                    find build/index.html
                    echo "Rulam testele scrise de dev"
                    npm run test
                '''
            }
        }
    }
}

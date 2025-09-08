pipeline {
    agent any

    stages {
        stage("Build"){
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                    args '-m 2g --cpus=1'
                }
            }
            steps{
                sh '''
                    ls -la
                    node --version
                    npm --version
                    echo "Ștergere node_modules și package-lock vechi"
                    rm -rf node_modules package-lock.json
                    npm install --prefer-offline
                    npm run build
                    ls -la
                '''
            }
        }
    }
}

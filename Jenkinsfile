pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '8b1b88bf-605b-4263-ba4e-8dbca71a5b9a'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

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
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
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
                    test -f build/index.html
                    npm test
                '''
            }
        }
        stage('deploy'){
             agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                    npm install netlify-cli 
                    node_modules/.bin/netlify --version
                    echo "deploying to: ${NETLIFY_SITE_ID}"
                    node_modules/.bin/netlify status
                '''
            }
        }
    }
}
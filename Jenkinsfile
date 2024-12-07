pipeline{
    agent any

    environment{
        NETLIFY_SITE_ID = '810a12f8-68eb-46c0-9a68-67b122216f5d'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {
        stage("Build") {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                    ls -la
                    npm ci
                    npm run build
                    ls -la
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
                        test -f build/index.html
                        npm test
                    '''
            }
        }

         stage("Deploy") {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify  --version
                    node_modules/.bin/netlify  status
                    node_modules/.bin/netlify  deploy --dir=build --prod
                '''
            }

        }

    }

 
}
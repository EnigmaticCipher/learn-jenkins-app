pipeline {
    agent any
    environment {
        NETLIFY_SITE_ID    = '1bac7265-97d5-4d11-affb-f07252f18b5d'
        NETLIFY_AUTH_TOKEN = credentials('netfily-token')
    }


    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:20-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    node --version
                    npm --version
                    npm install
                    npm run build
                '''
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:20-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    test -f build/index.html
                    ls -la
                '''
            }
        }

        stage('Deploy') {
            agent {
                docker {
                    image 'node:22-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    test -n "$NETLIFY_AUTH_TOKEN" && echo "token: défini" || echo "token: VIDE"
                    echo "site id: $NETLIFY_SITE_ID"
                    npm install netlify-cli --no-save
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build --prod
                '''
            }
        }
    }

    post {
        always {
            junit allowEmptyResults: true, testResults: '**/junit.xml'
        }
    }
}
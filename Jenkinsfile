pipeline {
    agent any
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
                npm test
                '''
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    post {
            always {
                junit 'test-results/junit.xml'
            }
        }
    }
}
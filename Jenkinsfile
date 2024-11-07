pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    echo List all file before build
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
        stage('Test'){
            parallel{
                stage('Unit'){
                    agent{
                        docker{
                            image 'node:18-alpine'
                            reuseNode true
                        }
                    }
                    steps{
                        sh '''
                            test -f public/index.html
                            npm test
                        '''
                    }
                }

                stage('E2E'){
                    agent{
                        docker {
                            image 'node:18-alpine'
                            reuseNode true
                        }
                    }
                    steps {
                        sh '''
                            echo E2E test command
                        '''
                    }
                }
            }
        } 
    }
    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}
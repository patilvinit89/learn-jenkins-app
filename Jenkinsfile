pipeline {
    agent any

    stages {
        // This is a comment
        /*
        comment1
        comment2
        */
        stage('Build') {
            agent {
                docker {
                    image 'node:20-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm -v
                    npm ci
                    npm run build
                    ls -la
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
                    echo "Test stage"
                    test -f build/index.html && echo "File exist" || echo "FIle doesn't exist"
		    npm test
                '''
            }
        }

        stage('E2E') {
	    agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.62.0-noble'
                    reuseNode true
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

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}

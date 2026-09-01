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
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}

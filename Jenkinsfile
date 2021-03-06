pipeline {
    agent { docker { image 'php' } }
    stages {
        stage('Build') {
            steps {
                sh 'echo "Build Complete!!"'
            }
        }
        stage('Test') {
            steps {
                sh 'php --version'
                sh 'tidy -q -e *.html'
            }
        }
        stage('Security Scan') {
            steps {
                aquaMicroscanner imageName: 'alpine:latest', notCompliesCmd: 'exit 1', onDisallowed: 'fail', outputFormat: 'html'
            }
        }
        stage('Deliver for development') {
            when {
                branch 'development'
            }
            steps {
                sh 'echo "Delivered to development!"'
                input message: 'Happy?'
                sh 'echo "Something else happened!"'
            }
        }
        stage('Deploy for production') {
            when {
                branch 'production'
            }
            steps {
                withAWS(region:'us-west-2',credentials:'aws-static') {
                    sh 'echo "Uploading content with AWS creds"'
                    s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'index.html', bucket:'playground-jenkins')
                }
            }
        }
    }
}

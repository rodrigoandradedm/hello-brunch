#!/usr/bin/env groovy
pipeline {
    agent any
    options {
        ansiColor('xterm')
    }
    stages {
        stage('Build') {
            steps {
                git branch: 'main', url: 'https://github.com/rodrigoandradedm/hello-brunch'
                sh 'docker-compose build'
                sh 'trivy filesystem -f json -o trivy-fs.json .'
                sh 'trivy image --format json --output trivy-results.json hello-brunch'           
            }
            post {
                always {
                    recordIssues enabledForFailure: true, tool: trivy(pattern: 'trivy-*.json')
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker-compose up -d' 
            }
        }
    }
}

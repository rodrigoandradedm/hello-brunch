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
            }
        }
        stage('Deploy')
            sh 'docker-compose up -d' 
        }
    }
}

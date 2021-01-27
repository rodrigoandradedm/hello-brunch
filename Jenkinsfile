#!/usr/bin/env groovy
pipeline {
    agent any
    options {
	ansicolor('xterm')
    }
    stages {
        stage('Build') {
            steps {
                sh 'docker-compose build'
                sh 'docker-compose up -d'
            }
        }
    }
}

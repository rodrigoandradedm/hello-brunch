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
	stage('Registry') {
            steps {
                withDockerRegistry([credentialsId:"gitlab-registry", url:"http://10.250.14.1:5050"]) {
                  sh 'docker tag hello-brunch:latest docker 10.250.14.1:5050/root/hello-brunch:latest'
                  sh 'docker push 10.250.14.1:5050/root/hello-brunch:latest'
                }
            }
        }
        //stage('Deploy') {
        //    steps {
        //        sh 'docker-compose up -d' 
        //    }
        //}
    }
}

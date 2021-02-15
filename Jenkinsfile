#!/usr/bin/env groovy
pipeline {
    agent any
    options {
        ansiColor('xterm')
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'docker-compose build'
            }
        }
	stage('Registry') {
            steps {
                withDockerRegistry([credentialsId:"gitlab-registry", url:"http://10.250.14.1:5050"]) {
                  sh 'docker tag hello-brunch:latest 10.250.14.1:5050/root/hello-brunch:BUILD-1.${BUILD_NUMBER}'
		  sh 'docker push 10.250.14.1:5050/root/hello-brunch:BUILD-1.${BUILD_NUMBER}'
		  sh 'docker tag hello-brunch:latest 10.250.14.1:5050/root/hello-brunch:latest'
		  sh 'docker push 10.250.14.1:5050/root/hello-brunch:latest'
		  sshagent (credentials: ['gitlabssh']) {
		    sh 'git push --tags'
		    sh 'git tag BUILD-1.${BUILD_NUMBER}'
		  }
                }
            }
        }
        stage('Deploy') {
          steps {
	    sh 'ssh -t -o "StrictHostKeyChecking no" deploy@10.250.14.1 "docker-compose pull && docker-compose up -d"'
	    sshagent (credentials: ['deployDocker']){
	        sh 'docker pull 10.250.14.1:5050/root/hello-brunch:BUILD-1.${BUILD_NUMBER}'
                //sh 'docker run -dit 10.250.14.1:5050/root/hello-brunch:BUILD-1.${BUILD_NUMBER}'
	    }
          }
        }
    }
}

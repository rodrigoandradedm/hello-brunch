#!/usr/bin/env groovy
pipeline {
    agent any
    options {
        ansiColor('xterm')
    }
    
    stages {
        stage('Build') {
            steps {
		sshagent (credentials: ['gitlabssh']) {
                  git branch: 'master', url: 'ssh://git@10.250.14.1:2224/root/hello-brunch.git'
		}
                sh 'docker-compose build'
            }
        }
	stage('Registry') {
            steps {
                withDockerRegistry([credentialsId:"gitlab-registry", url:"http://10.250.14.1:5050"]) {
                  sh 'docker tag hello-brunch:latest 10.250.14.1:5050/root/hello-brunch:BUILD-1.${BUILD_NUMBER}'
		  //sh 'docker tag hello-brunch:latest 10.250.14.1:5050/root/hello-brunch:latest'
		  sh 'git tag BUILD-1.${BUILD_NUMBER}'
                  sh 'docker push 10.250.14.1:5050/root/hello-brunch:BUILD-1.${BUILD_NUMBER}'
		  //sh 'docker push 10.250.14.1:5050/root/hello-brunch:latest'
		  sshagent (credentials: ['gitlabssh']) {
		    sh 'git push --tags http://10.250.14.1:8929/root/hello-brunch'
		  }
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

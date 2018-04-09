#!/usr/bin/env groovy

pipeline {
	agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                sh "/usr/local/maven/bin/mvn -f /var/lib/jenkins/workspace/pipe/RMM_APP/Services clean install " 
				sh "/usr/local/maven/bin/mvn -f $WORKSPACE clean install " 
            }
        }
        stage('ImageCreation'){
			steps {
				echo 'Dockerizing application ...'
				sh "/usr/bin/docker build -t java_docker_image ."
				sh "/usr/bin/docker save -o jenkinsimage.tar java_docker_image"
			}
        }
        stage('Deployment'){
			steps{
				echo 'Deploying docker application on Dev Server...'
				sh "scp jenkinsimage.tar root@10.30.124.23:/root/docker"
                sh "ssh root@10.30.124.23 /usr/bin/docker load -i /root/docker/jenkinsimage.tar"
                sh "ssh root@10.30.124.23 /usr/bin/docker run -i -t -d -p 8080:8080 java_docker_image"
                }
            } 
        }
    }
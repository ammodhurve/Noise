pipeline {
	agent{
	label 'slave-AZ’
	}
	parameters {
		choice(name: 'ENVIRONMENT', choices: ['QA','UAT'], description: 'Pick Environment value')
	}
	stages {
	    stage('Checkout') {
	        steps {
			checkout scm			       
		      }}
		stage('Build') {
	           steps {
			  sh '/home/ammo/slaveAZ/apache-maven-3.9.1/bin/mvn install’
	                 }}
		stage('Deployment'){
		    steps {
			sh 'sshpass -p "dev" scp target/LoginWebAppApplicationWith-Docker.war ammo@172.17.0.2:/home/ammo/slaveAZ/apache-tomcat-9.0.73/webapps
			}}
		stage('Docker build'){
		    steps {
			sh 'docker build -t swapnilhub/pipelineimage11.1.2 .'
			}}
		stage('Docker Login'){
		    steps {
		withCredentials([string(credentialsId: 'ammodhurve', variable: 'docker-amrita')]){
                sh ‘docker login -u amritahub -p${docker-amritahub}'                 
			echo 'Login Completed'
			}
			
			}}
		stage('Push Image to Docker Hub') {         
    		    steps{                            
 			sh 'docker push amritahub/pipelineimage11.1.2:$BUILD_NUMBER'           
			echo 'Push Image Completed'       
    			}}
	}}
		

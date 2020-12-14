pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
        stage('docker') {
        	steps {
            	withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: '9704605380ganna', usernameVariable: 'gowthamatr')]) {
            	sh "mvn spring-boot:build-image -Dspring-boot.build-image.imageName=gowthamatr/docker201"
        		}
        	}
        }
    }
}
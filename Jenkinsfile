pipeline {
    agent any
    environment {
        USER_CREDENTIALS = credentials('USER_PASSWORD')
    }
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
        stage('DockerBuild') {
        	steps {
                sh "export DOCKER_HOST='/var/run/docker.sock'"
            	sh "mvn spring-boot:build-image -Dspring-boot.build-image.imageName=gowthamatr/docker201"
        	}
        }
        
        stage('DockerPush') {
                def dockerImage = docker.image("gowthamatr/docker201")
                docker.withRegistry('https://registry.hub.docker.com', 'docker') {
                    dockerImage.push()
        		}
        }
    }
}

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
        stage('DockerBuild') {
        	steps {
            	withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: '9704605380ganna', usernameVariable: 'gowthamatr')]) {
                sh "export DOCKER_HOST='/var/run/docker.sock'"
            	sh "mvn spring-boot:build-image -Dspring-boot.build-image.imageName=gowthamatr/docker201"
        		}
        	}
        }
        stage('DockerPush') {
        	steps {
            	withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: '9704605380ganna', usernameVariable: 'gowthamatr')]) {
                sh "sudo docker login -u $usernameVariable -p $passwordVariable"
            	sh "sudo docker push $usernameVariable/docker201"
        		}
        	}
        }
    }
}

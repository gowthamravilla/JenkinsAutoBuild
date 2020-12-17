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
        stage('Docker Build') {
        	steps {
                sh "export DOCKER_HOST='/var/run/docker.sock'"
            	sh "mvn spring-boot:build-image -Dspring-boot.build-image.imageName=gowthamatr/docker201"
        	}
        }
        
        stage('Docker Push') {
            steps{
                script{
                def dockerImage = docker.image("gowthamatr/docker201")
                docker.withRegistry('https://registry.hub.docker.com', 'docker') {
                    dockerImage.push()
        		    }
                }
            }
        }
        stage('Docker Pull') {
            steps{
                script{
                def dockerImage = docker.image("gowthamatr/docker201")
                docker.withRegistry('https://registry.hub.docker.com', 'docker') {
                    dockerImage.pull()
        		    }
                }
            }
        }
        stage('Docker Run') {
            steps{
                script{
                        docker.image('gowthamatr/docker201').withRun('-p 9191:9191') {c ->
                        sh "curl -i http://${hostIp(c)}:9191/"
                     }
                }
            }
        }
    }
}
def hostIp(container) {
    sh "docker inspect -f {{.Node.Ip}} ${container.id} > hostIp"
    readFile('hostIp').trim()
}

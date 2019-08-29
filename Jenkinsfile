pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {

        stage('Initialize'){
                def dockerHome = tool 'mydocker'
                env.PATH = "${dockerHome}:${env.PATH}"
        }

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
         stage('Publish') {
             steps {
                echo 'Starting to build docker image!!'

                script {
                     // withDockerRegistry([credentialsId: 'shubhamsingh0', url: 'docker.io/shubhamsingh0']) {
                        def customImage = docker.build("shubhamsingh0/eureka_stock_discovery_service:latest")
                        customImage.push()
                    // }
                  
                }
            }
        }
    }
}

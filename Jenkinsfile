pipeline {
    agent any

    stages {

        stage('Build WAR') {
            steps {
                dir('/home/ubuntu/smartclinic') {
                    sh 'mvn clean package'
                }
            }
        }

        stage('Verify WAR') {
            steps {
                sh 'test -f /home/ubuntu/smartclinic/target/smartclinic.war'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker restart smartclinic-tomcat
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                    sleep 10
                    curl -f http://localhost:8081/smartclinic/
                '''
            }
        }
    }

    post {
        success {
            echo 'SmartClinic deployment successful'
        }

        failure {
            echo 'SmartClinic deployment failed'
        }
    }
}

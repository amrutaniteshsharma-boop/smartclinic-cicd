pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Source code checked out from GitHub'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Verify WAR') {
            steps {
                sh '''
                    test -f target/smartclinic.war
                    echo "WAR artifact created successfully"
                    ls -lh target/smartclinic.war
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    echo "Deploying SmartClinic WAR..."

                    docker cp target/smartclinic.war \
                    smartclinic-tomcat:/usr/local/tomcat/webapps/smartclinic.war

                    docker restart smartclinic-tomcat
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                    echo "Waiting for Tomcat..."
                    sleep 10

                    curl -fsS http://localhost:8081/smartclinic/ \
                    | grep -q "Status: Healthy"

                    echo "SmartClinic health check passed"
                '''
            }
        }
    }

    post {
        success {
            echo 'SmartClinic CI/CD deployment successful'
        }

        failure {
            echo 'SmartClinic CI/CD deployment failed'
        }
    }
}

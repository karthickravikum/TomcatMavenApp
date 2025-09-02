pipeline {
    agent any

    tools {
        maven 'M3'   // Use the Maven installation configured in Jenkins
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the project with Maven...'
                sh 'mvn -v'             // Verify Maven version
                sh 'mvn clean package'   // Build WAR
            }
        }

        stage('Deploy') {
            steps {
                echo 'Stopping any existing container...'
                sh 'docker stop C1 || true'
                sh 'docker rm C1 || true'

                echo 'Copying WAR to /tmp...'
                sh 'cp target/*.war /tmp/app.war'

                echo 'Starting Tomcat container with new WAR...'
                sh '''
                   docker run -d -p 8001:8080 \
                   -v /tmp/app.war:/usr/local/tomcat/webapps/app.war \
                   --name C1 tomcat:latest
                '''
            }
        }
    }

    post {
        success {
            echo 'Build and deployment completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for errors.'
        }
    }
}

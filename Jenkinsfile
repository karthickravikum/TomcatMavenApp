pipeline {
    agent any
    tools {
        maven 'M3'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/karthickravikum/TomcatMavenApp.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker stop C1 || true'
                sh 'docker rm C1 || true'
                sh 'cp target/TomcatMavenApp-2.0.war /tmp/'
                sh 'docker run -d -p 8001:8080 -v /tmp/TomcatMavenApp-2.0.war:/usr/local/tomcat/webapps/TomcatMavenApp-2.0.war --name C1 tomcat:latest'
            }
        }
    }
}

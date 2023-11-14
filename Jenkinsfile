pipeline {
    agent any

    parameters {
        string(name: 'GIT_URL', description: 'Git repository URL', defaultValue: 'https://github.com/your/repo.git')
        string(name: 'TOMCAT_URL', description: 'Tomcat server URL (e.g., http://tomcat-server:8080)', defaultValue: 'http://localhost:8080')
        string(name: 'TOMCAT_USERNAME', description: 'Tomcat manager username', defaultValue: 'admin')
        password(name: 'TOMCAT_PASSWORD', description: 'Tomcat manager password', defaultValue: 'admin')
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Retrieve Git URL from parameters
                    def gitUrl = params.GIT_URL

                    // Checkout the source code from the specified Git repository
                    git url: gitUrl
                }
            }
        }

        stage('Build') {
            steps {
                // Build your Maven project
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    def tomcatUrl = params.TOMCAT_URL
                    def tomcatUsername = params.TOMCAT_USERNAME
                    def tomcatPassword = params.TOMCAT_PASSWORD

                    // Deploy the WAR file to Tomcat using the manager API
                    sh "curl --upload-file target/*.war --user ${tomcatUsername}:${tomcatPassword} ${tomcatUrl}/myapp"
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}

pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Skip tests during the build to speed up the process
                sh 'mvn -DskipTests clean package'
            }
        }

        stage('Test') {
            steps {
                // Run tests separately
                sh 'mvn test'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                // Deploy the packaged WAR file to Tomcat
                script {
                    deploy adapters: [
                        tomcat9(
                            credentialsId: 'f04f100b-2530-411e-ad70-722a6b16730e',
                            path: '',
                            url: 'http://ec2-3-149-255-75.us-east-2.compute.amazonaws.com:8080/'
                        )
                    ],
                    contextPath: 'javawebapp',
                    war: '**/java-web-project.war'
                }
            }
        }
    }

    post {
        success {
            echo 'Build, Test and Deployment completed successfully.'
        }
        failure {
            echo 'Error during one of the stages.'
        }
    }
}

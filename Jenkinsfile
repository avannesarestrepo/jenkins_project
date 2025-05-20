pipeline {
    agent any

    tools {
        maven 'Maven 3.9'
        jdk 'JDK 17'
    }

    environment {
        BUILD_ARTIFACT = "target/jenkins_project-0.0.1-SNAPSHOT.jar"
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/avannesarestrepo/jenkins_project.git', branch: 'master'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests=false'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: "${BUILD_ARTIFACT}", fingerprint: true
            }
        }
    }

    post {
        always {
            junit '**/target/surefire-reports/*.xml'
        }

        failure {
            mail to: 'avannesarestrepo@gmail.com',
                 subject: "Pipeline Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Check Jenkins for details: ${env.BUILD_URL}"
        }
    }
}
pipeline {
    agent any

    tools {
        jdk 'jdk-17'         // <-- must match exactly
        maven 'Maven 3'      // <-- must match exactly
    }

    environment {
        SONAR_SCANNER_HOME = tool 'SonarScanner'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/vinay4729/simple-java-maven-app.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Unit Test Report') {
            steps {
                junit 'target/surefire-reports/*.xml'
            }
        }

        stage('SonarQube Analysis') {
            when {
                branch 'master'
            }
            steps {
                withSonarQubeEnv('MySonar') {
                    sh 'mvn sonar:sonar -Dsonar.projectKey=simple-java-maven-app'
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'target/*.jar', allowEmptyArchive: true
        }
    }
}


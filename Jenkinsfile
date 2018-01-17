pipeline {
    agent any
    stages {
        stage('SCM') {
            checkout scm
        }
        stage('SonarQube analysis') {
            //def sonarqubeScannerHome = tool name: 'sonar_scanner'
            def sonar_scanner = tool name: 'sonar_scanner';
            withSonarQubeEnv('sonar_service') {
                //sh "${sonarqubeScannerHome}/bin/sonar-scanner"
                sh "${sonar_scanner}/bin/sonar-scanner"
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
                 //sh 'mvn -f pom.xml clean compile sonar:sonar'
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
    }
}

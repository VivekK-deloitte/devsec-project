pipeline {
    agent any
    
    tools {
        maven "Maven 3.8.6"
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests=true'
                archive 'target/*.jar'
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
        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('SonarQube'){
                    sh "mvn sonar:sonar -Dsonar.projectKey=maven-project -Dsonar.host.url=http://localhost:9000"
                }
            }
    }
    }
}

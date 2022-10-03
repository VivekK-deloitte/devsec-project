pipeline {
    agent any
    
    tools {
        maven "Maven 3.8.6"
    }
    environment{
    dockerHub=credentials('62b178b5-4de3-42dd-b76a-99de4aa1ee84')
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
         stage('Docker Build') {
            steps {
                sh "docker build -t myimage:v1 ."
            }
        }
        stage('Docker Push') {
            steps {
                sh "docker tag myimage:v1 vivek02deloitte/hu_docker"
                sh "echo $dockerHub_PSW | docker login -u $dockerHub_USR --password-stdin"
                sh "docker push vivek02deloitte/hu_docker"
            }
        }
        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('SonarQube'){
                    sh "mvn clean verify sonar:sonar \
  -Dsonar.projectKey=maven-demo-jenkins \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=sqp_adcc85eb16d46631127e131fae21158737b441ba"
                }
            }
    }
    }
}

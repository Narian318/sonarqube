pipeline{
    agent any
    tools {
        maven 'maven3'
    }
    stages{
        stage("SCM"){
            steps{
                git 'https://github.com/Narian318/sonarqube.git'
            }
        }
        stage("Build Artifact") {
            steps{
                sh "mvn clean package"
            }
        }
        stage("Deploy to Sonar") {
            steps{
                withSonarQubeEnv(installationName: 'sonar-8', credentialsId: 'jenkins-token') {
                    sh "${ tool ("sonar-scanner")}/sonar-scanner -Dsonar.projectKey=hellospringboot -Dsonar.projectName=hellospringboot -Dsonar.sourceEncoding=UTF-8 -Dsonar.sources=src"
                }
            }
        }
	stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}

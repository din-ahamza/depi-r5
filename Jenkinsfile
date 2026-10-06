pipeline {
    agent {
        label "agent-01"

    }
    tools {
        jdk 'jdk-11'
        maven 'maven-3-5-4'
    }
    environment {
        dockerUsername = credentials("docker-username")
        dockerPass = credentials("docker-passwd")
    }
    stages {
        stage("Build Java App") {
            steps {
                sh "mvn package install -DskipTests=true"
            }
        }
        stage("Test Java App") {
            steps {
                sh "mvn test"
            }
        }
        stage("Build Docker Image") {
            steps {
                sh "docker build -t dinahamza/depi-java:v${BUILD_NUMBER} ."
            }
        }
        stage("Login into DockerHub") {
            steps {
                sh "docker login -u ${dockerUsername} -p ${dockerPass}"
            }
        }
        stage("Push Docker Image") {
            steps {
                sh "docker push dinahamza/depi-java:v${BUILD_NUMBER}"
            }
        }
        
    }
}
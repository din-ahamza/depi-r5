@Library('depi-lib') _

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
                script{
                    def mvn = new edu.depi.maven()
                    mvn.mavenCommand("package install -DskipTests=true")
                }
            }
        }
        stage("Test Java App") {
            steps {
                script{
                    def mvn = new edu.depi.maven()
                    mvn.mavenCommand("test")
                }
            }
        }
        stage("Build Docker Image") {
            steps {
                script{
                    def dockerFun  = new edu.depi.docker()
                    dockerFun.dockerBuild("dinahamza/depi-java", "v${BUILD_NUMBER}")
                }
            }
        }
        stage("Login into DockerHub") {
            steps {
                script{
                    def dockerFun  = new edu.depi.docker()
                    dockerFun.dockerLogin("${dockerUsername}", "${dockerPass}")
                }
            }
        }
        // stage("Push Docker Image") {
        //     steps {
        //         sh "docker push dinahamza/depi-java:v${BUILD_NUMBER}"
        //     }
        // }
        
    }
}
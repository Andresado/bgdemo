#!groovy

pipeline {
    agent none
    options {
        timeout(time: 1, unit: 'DAYS')
        disableConcurrentBuilds()
    }
    stages {
        stage("Build and Register Image") {
            agent any
            steps { buildAndRegisterDockerImage() }
        }
    }
}

def initialize() {
    env.REGISTRY_URL = "http://localhost:32769/"
    env.REGISTRY_CREDENTIALS = "andresado1383"
    env.IMAGE_NAME = "bgdemo"
}

def buildAndRegisterDockerImage() {
    def buildResult
    docker.withRegistry("${env.REGISTRY_URL}","${env.REGISTRY_CREDENTIALS}") {
        echo "Building ${env.IMAGE_NAME}"
        buildResult = docker.build("${env.IMAGE_NAME}:${env.BUILD_ID}")
        echo "Register ${env.IMAGE_NAME} at ${env.REGISTRY_URL}"
        buildResult.push("${env.BUILD_ID}")
        echo "Disconnect from registry"
        sh "docker logout ${env.REGISTRY_URL}"
    }
}

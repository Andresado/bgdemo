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
    env.REGISTRY_URL = "https://registry-1.docker.io"
    env.REGISTRY_CREDENTIALS = "andresado1383"
    env.IMAGE_NAME = "bgdemo"
}

def buildAndRegisterDockerImage() {
    echo "Ingreso"
    def buildResult
    docker.withRegistry(env.REGISTRY_URL) {
        echo "Build ${env.IMAGE_NAME}"
        buildResult = docker.build(env.IMAGE_NAME)
//      buildResult = docker.build(env.IMAGE_NAME:env.BUILD_ID)
        echo "Register ${env.IMAGE_NAME} at ${env.REGISTRY_URL}"
        buildResult.push()
        echo "Disconnect from registry"
        sh "docker logout ${env.REGISTRY_URL}"
    }
}

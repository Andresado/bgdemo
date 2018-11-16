#!groovy

pipeline {
    agent none
    options {
        timeout(time: 10, unit: 'MINUTES')
    }
    stages {
      stage("Init") {
          agent any
          steps { initialize() }
      }
        stage("Build and Register Image") {
            agent any
            steps { buildAndRegisterDockerImage() }
        }
  	}
}
def initialize() {
    env.REGISTRY_URL = "http://169.62.240.152:8083"
    env.REGISTRY_CREDENTIALS = "registry"
    env.IMAGE_NAME = "bgdemo"
}

def buildAndRegisterDockerImage() {
    def buildResult
    docker.withRegistry("${env.REGISTRY_URL}") {
        echo "Building ${env.IMAGE_NAME}"
        buildResult = docker.build("${env.IMAGE_NAME}:${env.BUILD_ID}")
        echo "Register ${env.IMAGE_NAME} at ${env.REGISTRY_URL}"
        buildResult.push("${env.BUILD_ID}")
        echo "Disconnect from registry"
      }
}

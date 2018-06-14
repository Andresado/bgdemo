#!groovy

pipeline {
    agent none
    options {
        timeout(time: 1, unit: 'DAYS')
        disableConcurrentBuilds()
    }
    stages {
//        stage("Init") {
//            agent any
//            steps { initialize() }
//        }
        stage("Build and Register Image") {
            agent any
            steps { buildAndRegisterDockerImage() }
        }
    }
}

//def initialize() {
//    env.REGISTRY_URL = "https://10.130.2.201:8080"
//    env.REGISTRY_CREDENTIALS = "nexus-qa"
//    env.IMAGE_NAME = "ucdbgdemo"
//    showEnvironmentVariables()
//}
//
//def showEnvironmentVariables() {
//    sh 'env | sort > env.txt'
//    sh 'cat env.txt'
//}

def buildAndRegisterDockerImage() {
    def buildResult
//  docker.withRegistry(env.REGISTRY_URL, env.REGISTRY_CREDENTIALS) {
    docker.withRegistry() {
//      echo "Build ${env.IMAGE_NAME}"
        echo "Build image"
//      buildResult = docker.build(env.IMAGE_NAME:env.BUILD_ID)
        buildResult = docker.build()
//      echo "Register ${env.IMAGE_NAME} at ${env.REGISTRY_URL}"
        echo "Register image at registry"
        buildResult.push()
//      echo "Disconnect from registry at ${env.REGISTRY_URL}"
//      sh "docker logout ${env.REGISTRY_URL}"
    }
}

#!groovy

pipeline {
    agent none
    options {
        timeout(time: 10, unit: 'MINUTES')
    }
    
        parameters {
        string( name: 'USER', defaultValue: '', description: 'Correo electronico')
        string( name: 'IMAGE_NAME', defaultValue: 'bgdemo', description: 'Identificador Servicio')
		string( name: 'IMAGE_NUMBER', defaultValue: '1', description: 'Identificador Imagen')
    }

    environment {
        REGISTRY_URL = "${REGISTRY_URL}"
        REGISTRY_URL_IP = "${REGISTRY_URL_IP}"
        REGISTRY_CREDENTIALS = "${REGISTRY_CREDENTIALS}"
	    IMAGE_NAME = "${IMAGE_NAME}"
		IMAGE_NUMBER = "${IMAGE_NUMBER}"		
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
    env.REGISTRY_URL = params["REGISTRY_URL"]
    env.REGISTRY_URL_IP = "REGISTRY_URL_IP"
    env.REGISTRY_CREDENTIALS = params["REGISTRY_CREDENTIALS"]
    env.IMAGE_NAME = params["IMAGE_NAME"]
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

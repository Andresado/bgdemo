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
        
        stage("EntregaUCD") {
	 agent any	
         steps { 

            step([$class: 'UCDeployPublisher',
        siteName: 'local',
        component: [
            $class: 'com.urbancode.jenkins.plugins.ucdeploy.VersionHelper$VersionBlock',
            componentName: 'bgdemoWebpage',
            createComponent: [
                $class: 'com.urbancode.jenkins.plugins.ucdeploy.ComponentHelper$CreateComponentBlock',
                componentTemplate: 'Kubernetes',
                componentApplication: 'Blue-Green Demo'
            ],
            delivery: [
                $class: 'com.urbancode.jenkins.plugins.ucdeploy.DeliveryHelper$Push',
                pushVersion: '${BUILD_NUMBER}',
                baseDir: "${workspace}/tmp/webpage",
                fileIncludePatterns: '**/*',
                fileExcludePatterns: '',
				pushProperties: "Ver_imagen=${env.BUILD_ID}",
            ]
        ]
    ])
        
        
        }
    }
	}
}
def initialize() {
    env.REGISTRY_URL = "http://192.168.1.64:5000"
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
        sh "docker logout ${env.REGISTRY_URL}"
    }
}

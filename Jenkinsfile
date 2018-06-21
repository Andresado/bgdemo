



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
                baseDir: 'jobs\\Demo_Fidelizacion\\tmp',
                fileIncludePatterns: '*',
                fileExcludePatterns: '',
                pushProperties: 'pushProperties',
                pushDescription: 'Pushed_app_kuber'
            ]
        ]
    ])
        
        
        }
    }
	}
def initialize() {
    env.REGISTRY_URL = "http://localhost:5000"
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
	}

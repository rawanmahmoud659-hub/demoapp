
@Library('shared-lib@main') _

pipeline {
    agent {
        label 'agent1'
    }

    options {
        skipDefaultCheckout(true)
        timestamps()
    }

    environment {
        DOCKER_IMAGE = 'rawanm21/demoapp'
        IMAGE_TAG = "${BUILD_NUMBER}"
        DEPLOYMENT_FILE = 'deployment.yaml'
        K8S_NAMESPACE = 'ivolve'
	SERVER = 'https://172.31.7.13:6443'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/rawanmahmoud659-hub/demoapp.git'
            }
        }

        stage('Build Application') {
            steps {
                script {
                    buildApp()
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    buildImage(
                        env.DOCKER_IMAGE,
                        env.IMAGE_TAG
                    )
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    deployOnK8s(
                        env.DOCKER_IMAGE,
                        env.IMAGE_TAG,
                        env.DEPLOYMENT_FILE,
			env.SERVER
                    )
                }
            }
        }
    }

    post {
        success {
            echo 'DemoApp deployed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}


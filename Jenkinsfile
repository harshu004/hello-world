def productionImage
def ACCOUNT_REGISTRY_PREFIX
def GIT_COMMIT_HASH
def registryCredential

pipeline {
    agent any
    stages {
        stage('Checkout Source Code and Logging Into Registry') {
            steps {
                echo 'Logging Into the Private ECR Registry'
                script {
                    GIT_COMMIT_HASH = sh (script: "git log -n 1 --pretty=format:'%H'", returnStdout: true)
                    ACCOUNT_REGISTRY_PREFIX = "harshu004"
                    registryCredential = 'dockerhub'
                }
            }
        }

        stage('Make A Builder Image') {
            steps {
                echo 'Starting to build the project builder docker image'
                script {
                    builderImage = docker.build("${ACCOUNT_REGISTRY_PREFIX}/docker-nodejs:${GIT_COMMIT_HASH}", "-f ./Dockerfile.builder .")
                    docker.withRegistry( '', registryCredential ) {
                    builderImage.push()
                    builderImage.push("${env.GIT_BRANCH}")
                    builderImage.inside('-v $WORKSPACE:/output -u root') {
                        sh """
                           cd /output
                           lein uberjar
                           lein test
                        """
                      }
                    }
                }
            }
        }
    }
}

//def kong_harbor = docker.image('oneapi/kong-oneapi:2.8.1-2.4.0');
def kong_harbor = docker.image('oneapi/kong-oneapi:2.8.1-2.6.0');
def kong_azure = docker.image('fusoeca.azurecr.io/kong-oneapi:2.8.1-2.6.0');
pipeline {
    options {
        timeout(time: 59, unit: 'MINUTES')
    }
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    cleanWs()
                    deleteDir()
                    checkout scm
                }
            }
        }

        stage("Pull & Push Image") {
            steps {
                retry(count: 2) {
                    script {
                        docker.withRegistry("https://registry.app.corpintra.net") {
                            kong_harbor.pull();
                        }
                        sh 'docker tag registry.app.corpintra.net/oneapi/kong-oneapi:2.8.1-2.6.0 fusoeca.azurecr.io/kong-oneapi:2.8.1-2.6.0'
                        
                        docker.withRegistry("https://fusoeca.azurecr.io", "fusoeca") {
                            kong_azure.push()
                        }
                    }
                }
            }
        }
    }

    post {
           always {
               cleanWs()
           }
   }
}

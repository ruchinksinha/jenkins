def kong_harbor = docker.image('oneapi/kong-oneapi:2.8.1-2.4.0');
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

        stage("Push Image") {
            steps {
                retry(count: 2) {
                    script {
                        docker.withRegistry("https://registry.app.corpintra.net") {
                            kong_harbor.pull();
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

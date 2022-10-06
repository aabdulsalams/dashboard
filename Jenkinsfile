pipeline {
    agent any

    environment {
        storage_endpoint = 'https://storage.googleapis.com/nanda-test/testing-micro-fe/'
        app_name_basic = '360-client-portal-frontend'
        server_endpoint = '10.148.0.61'
        server_credential = 'automation-ssh'
    }

    stages {
        stage("Set Variables") {
            steps {
                script {
                    // ganti sementara master => main
                    if ("${env.BRANCH_NAME}" == "main") {
                        env.service_dir = "dashboard/prod"
                    }
                }
            }
        }
        stage("Build Application") {
            steps {
                script {
                    sh "npm run build"
                }
            }
        }
        stage("Store Builded Application To Cloud Storage") {
            steps {
                script {
                    //https://plugins.jenkins.io/google-storage-plugin/
                }
            }
        }
    }
    
    post {
        always {
            echo "Release finished do cleanup"
            deleteDir()
        }
        success {
            echo "Release Success"
        }
        failure {
            echo "Release Failed"
        }
        cleanup {
            echo "Clean up in post work space"
            cleanWs()
        }
    }
}

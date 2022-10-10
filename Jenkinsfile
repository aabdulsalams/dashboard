pipeline {
    agent any

    environment {
        storage_endpoint = 'https://storage.googleapis.com/nanda-test/testing-micro-fe/'
        app_name_basic = '360-client-portal-frontend'
        server_endpoint = '10.148.0.61'
        server_credential = 'automation-ssh'
        file_target = 'webpack.config.js'
    }
    // mulit source => https://stackoverflow.com/questions/14843696/checkout-multiple-git-repos-into-same-jenkins-workspace
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
//         stage("Build Application") {
//             steps {
//                 script {
//                     sh "npm run build"
//                 }
//             }
//         }
        stage("Store Builded Application To Cloud Storage") {
            steps {
                script {
                    sh 'echo "MASUKKKKKK"'
                    //https://plugins.jenkins.io/google-storage-plugin/
                    // If we name pattern build_environment.txt, this will upload build_environment.txt to our GCS bucket.
                    step([$class: 'ClassicUploadStep', credentialsId: 'adtech-cloud-storage',  bucket: "gs://${env.storage_endpoint}",
                          pattern: env.file_target])
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

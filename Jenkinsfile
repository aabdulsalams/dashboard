pipeline {
    agent any

    environment {
        basic_storage_endpoint = 'nanda-test/testing-micro-fe'
        app_name_basic = '360-client-portal-frontend'
    }
    
    // mulit source => https://stackoverflow.com/questions/14843696/checkout-multiple-git-repos-into-same-jenkins-workspace
    stages {
        stage("Set Variables") {
            steps {
                script {
                    env.feature_name = scm.getUserRemoteConfigs()[0].getUrl().tokenize('/').last().split("\\.")[0]
                    echo "${testing}"
                    // ganti sementara master => main
                    if ("${env.BRANCH_NAME}" == "main") {
                        env.service_dir = "dashboard/prod"
                        env.storage_endpoint = "${basic_storage_endpoint}/${feature_name}/prod"
                    }
                    else if ("${env.BRANCH_NAME}" == "testing") {
                        env.service_dir = "dashboard/testing"
                        env.storage_endpoint = "${basic_storage_endpoint}/${feature_name}/testing"
                    }
                    else if ("${env.BRANCH_NAME}" == "release") {
                        env.service_dir = "dashboard/prerelase"
                        env.storage_endpoint = "${basic_storage_endpoint}/${feature_name}/prerelease"
                    }
                    else {
                        env.service_dir = "dashboard/unknown"
                        env.storage_endpoint = "${basic_storage_endpoint}/${feature_name}/unknown"
                    }
                }
            }
        }
        https://plugins.jenkins.io/nodejs/
        stage("Build Application") {
            steps {
                script {
                    sh "node --version"
//                     sh "npm run build"
                }
            }
        }
        stage("Store Builded Application To Cloud Storage") {
           steps {
               withCredentials([file(credentialsId: 'cloud-storage-object-admin', variable: 'GC_KEY')]) {
                   sh "gcloud auth activate-service-account --key-file=${GC_KEY}"
                   sh "gsutil -m cp -R src gs://${storage_endpoint}"
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

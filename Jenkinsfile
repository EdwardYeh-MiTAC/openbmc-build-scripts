pipeline {
    agent any

    environment {
//        GIT_REPO_URL = 'https://micgitlab1.mic.com.tw/beoc/solution-enabling/openbmc/openbmc.git'
        GIT_REPO_URL = 'ssh://git@micgitlab1-ssh.mic.com.tw:1022/beoc/solution-enabling/openbmc/openbmc.git'
        GIT_BRANCH = 'mitac_project_argus'
        GIT_CREDENTIALS_ID = 'micgitlab-eyeh'
        BUILD_MACHINE = 'ubuntu:24.04' 
        CLOUD_STORAGE_PATH = '/data/cloud-storage/' 
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: env.GIT_BRANCH]],
                        userRemoteConfigs: [[
                            url: env.GIT_REPO_URL,
                            credentialsId: env.GIT_CREDENTIALS_ID
                        ]]
                    ])
                }
            }
        }

        stage('Setup Build Environment') {
            steps {
                script {
                    //sh 'sudo apt update && sudo apt install -y build-essential python3 python3-pip git'
                    sh 'sudo apt update'
                }
            }
        }

        stage('Build OpenBMC') {
            steps {
                script {
                    sh '''
                    source oe-init-build-env
                    bitbake obmc-phosphor-image
                    '''
                }
            }
        }

        stage('Upload to Cloud') {
            steps {
                script {
                    sh '''
                    mkdir -p ${CLOUD_STORAGE_PATH}
                    cp build/tmp/deploy/images/*/*.wic ${CLOUD_STORAGE_PATH}
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Build and upload completed successfully!'
        }
        failure {
            echo 'Build failed! Please check logs.'
        }
    }
}


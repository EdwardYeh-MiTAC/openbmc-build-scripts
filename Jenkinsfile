pipeline {
    agent {
        docker {
            image 'ubuntu:22.04'
            args '-u root'
        }
    }
    stages {
        stage('Checkout') {
            steps {
                // git branch: 'main', url: 'https://github.com/EdwardYeh-MiTAC/openbmc-build-scripts.git'
                // git credentialsId: 'micgitlab-eyeh', url: 'https://micgitlab1.mic.com.tw/beoc/solution-enabling/openbmc/openbmc.git', branch: 'mitac_project_argus'
                git branch: '2.17.0-dev_B8261', url: 'https://github.com/EdwardYeh-MiTAC/openbmc.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                // sh 'apt-get update && apt-get install -y git build-essential'
                sh 'apt-get update && apt-get install -y git'
            }
        }
        stage('Build OpenBMC') {
            steps {
                sh 'export http_proxy="http://10.99.15.181:3128" && source setup b8261 && time bitbake obmc-phosphor-image && ls tmp/deploy/images/b8261/image-*'
            }
        }
        stage('Publish Artifacts') {
            steps {
                archiveArtifacts artifacts: 'tmp/deploy/images/b8261/*'
            }
        }
    }
}


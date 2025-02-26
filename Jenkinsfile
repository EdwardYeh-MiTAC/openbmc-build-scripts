pipeline {
    agent any

    environment {
        WORKSPACE = "workspace"
    }

    stages {
        stage('Clone OpenBMC Repository') {
            steps {
//                git branch: 'main',
//                    credentialsId: 'your-credentials-id',
//                    url: 'https://github.com/openbmc/openbmc.git'
//                git branch: '2.17.0-dev_B8261', url: 'https://github.com/EdwardYeh-MiTAC/openbmc.git'
                git branch: 'mitac_project_argus',
                    credentialsId: 'micgitlab-eyeh',
                    url: 'https://micgitlab1.mic.com.tw/beoc/solution-enabling/openbmc/openbmc.git'
            }
        }
        stage('Checkout Build Scripts') {
            steps {
//                git branch: 'main',
//                    credentialsId: 'your-credentials-id',
//                    url: 'https://github.com/openbmc/openbmc-build-scripts.git',
                git branch: 'mitac_project_argus', 
                    url: 'https://github.com/EdwardYeh-MiTAC/openbmc-build-scripts.git',
                    changelog: false,
                    poll: false
            }
        }
        stage('Build OpenBMC') {
            steps {
                sh '''
                    export LANG=en_US.UTF8
                    cd "${WORKSPACE}"
                    if [ -d openbmc ]; then
                        git -C openbmc fetch
                        git -C openbmc rebase
                    else
                        git clone https://edward.yeh:glpat-XWSNqanryGTScXwEXWXR@micgitlab1.mic.com.tw/beoc/solution-enabling/openbmc/openbmc.git
                    fi
                    export build_dir="${WORKSPACE}/build"
                    # Run your build script here
                    "${WORKSPACE}/openbmc-build-scripts/build-setup.sh"
                '''
            }
        }
        stage('Upload Build Artifacts') {
            steps {
                // Upload artifacts to a repository or cloud storage
                // Example using Artifactory or similar
                def server = Artifactory.server "10.99.15.181"
//                def uploadSpec = '''{ "files": [ { "pattern": "build/*.tar", "target": "libs-snapshot-local" } ] }'''
//                def buildInfo = server.upload spec: uploadSpec
            }
        }
    }
    post {
        success {
            echo 'Build and upload successful'
        }
        failure {
            echo 'Build or upload failed'
        }
    }
}


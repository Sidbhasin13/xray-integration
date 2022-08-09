pipeline {
    stages {
        when {
          branch 'main'
      }
        stage('Xray Initialization'){
            steps{
                script {
                    rtServer = Artifactory.newServer url: 'http://localhost:8082/', username: "admin" , password: "%40%24ID%26bHA786"
                    buildInfo = Artifactory.newBuildInfo()
                }
            }
        }

        stage('Build') {
            steps {
                script {
                        echo "PERFORM BUILD AND UPLOAD TO ARTIFACTIORY HERE"
                        buildName: env.JOB_NAME,
                        buildNumber: BUILD_NUMBER
                    )
                }
            }
        }

        stage('Configure Xray build info'){
            steps{
                rtBuildInfo (
                    buildName: "my-build",
                    buildNumber: BUILD_NUMBER,
                    maxBuilds: 1,
                    maxDays: 2,
                    doNotDiscardBuilds: ["3"],
                    deleteBuildArtifacts: true
                )
            }
        }

        stage('Publish to Xray'){
            steps {
                rtPublishBuildInfo (
                    serverId : 'server',
                    buildName : env.JOB_NAME,
                    buildNumber : BUILD_NUMBER
                )
            }
        }

        //Scan Build Artifacts in Xray
        stage('Xray Scan') {
            steps{
                script{
                   xrayScan (
                        serverId :   "server",
                        buildName    : env.JOB_NAME,
                        buildNumber : BUILD_NUMBER,
                        failBuild    : false
                    )
                }
            }
        }
    }
}

pipeline {
    options {
        timeout(time: 600, unit: 'MINUTES')  // timeout for the Job (usually it runs around 10 minutes)
    }

    agent {
        label 'ec2_build'
    }

    libraries {
       lib('nr-jenkins-libs')
    }

    stages {
        stage('Build') {
            steps {
                sh "make -C cni-plugin image ARCH=arm64"
                script {
                    ARTIFACT_VERSION = nrArtifactVersion()
                    println("Version = " + ARTIFACT_VERSION)
		}

		sh "docker tag cni:latest-arm64 ${CIG_CONTAINER_REGISTRY_PFX_DEV}/aspen/calico-cni-plugin:${ARTIFACT_VERSION}"
            }
        }

        stage('Push') {
            steps {
                nrPushContainer(
                    image: "${CIG_CONTAINER_REGISTRY_PFX_DEV}/aspen/calico-cni-plugin",
                    tag: "${ARTIFACT_VERSION}")
            }
        }
   }
}

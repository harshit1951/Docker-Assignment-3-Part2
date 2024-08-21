pipeline {
  agent any
  environment {
        DOCKER_IMAGE = 'maven-img'
        DOCKER_TAG = 'v1'
	      DOCKER_VOLUME = 'my-vol'
	      DOCKER_CONTAINER = 'my-maven-app'
  }
  tools {
      maven "Maven_Version"
      dockerTool 'Docker'
  }

  stages {
        stage('Extract Mount Path and Copy Files') {
            steps {
                script {
                    // Extract the mount path
                    def mountPath = sh(script: "docker volume inspect my-vol -f '{{ .Mountpoint }}'", returnStdout: true).trim()
                    echo "Mount Path: ${mountPath}"

                    // Copy files from the volume to the Jenkins workspace
                    sh "cp -r ${mountPath}/* ${WORKSPACE}/"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t my-app ."
            }
        }
    }  
}

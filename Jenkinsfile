pipeline {
  agent any

  environment {
    IMAGE_NAME = 'flask-app'
    IMAGE_TAG = 'latest'
    IMAGE_FULL_NAME = "${IMAGE_NAME}:${IMAGE_TAG}"
    IMAGE_TAR_PATH = "${env.HOME}/page/${IMAGE_NAME}.tar"
  }

  stages {
    stage('📥 Clone repository') {
      steps {
        git url: 'https://github.com/LFPOA/hello_world_flask.git', branch: 'main'
      }
    }
    stage('🐳 Build image with nerdctl') {
      steps {
        sh """
          sudo nerdctl build -t ${IMAGE_FULL_NAME} .
        """
      }
    }
    stage('💾 Save image to 바탕화면') {
      steps {
        sh """
          mkdir -p "${IMAGE_TAR_PATH%/*}"
          sudo nerdctl save -o "${IMAGE_TAR_PATH}" ${IMAGE_FULL_NAME}
        """
      }
    }
  }
}

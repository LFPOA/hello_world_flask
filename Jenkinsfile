pipeline {
  agent any

  parameters {
    choice(
      name: 'ENV',
      choices: ['dev', 'stage', 'prod'],
      description: '배포 대상 K8s 네임스페이스(테스트용)'
    )
  }
  
  environment {
  IMAGE_NAME = 'flask-k8s'
  IMAGE_TAG = 'latest'
  IMAGE_FULL_NAME = "${IMAGE_NAME}:${IMAGE_TAG}"
  IMAGE_DIR_PATH = "${env.HOME}/page"
  IMAGE_TAR_PATH = "${IMAGE_DIR_PATH}/${IMAGE_NAME}.tar"
  DEPLOYMENT_YAML = 'flask-deployment.yaml'
  SERVICE_YAML = 'flask-service.yaml'
}

  stages {
    stage('📥 Clone repository') {
      steps {
        git url: 'https://github.com/LFPOA/hello_world_flask.git', branch: 'main'
      }
    }
    //stage('🐳 Build image with nerdctl') {
      //steps {
        //sh """
          //sudo nerdctl build -t ${IMAGE_FULL_NAME} .
        //"""
      //}
    //}
    //stage('💾 Save image to page') {
      //steps {
        //sh """
          //mkdir -p "${IMAGE_DIR_PATH}"
          //sudo nerdctl save -o "${IMAGE_TAR_PATH}" ${IMAGE_FULL_NAME}
        //"""
        //echo "✅ 이미지가 저장됨: ${IMAGE_TAR_PATH}"
      //}
    //}
    stage('🚀 Deploy to Kubernetes(${params.ENV})') {
      steps {
        // admin.conf를 Jenkins에 secret file credential로 등록했다고 가정 (ID: kube-admin-conf)
        withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
          sh """
            export KUBECONFIG=$KUBECONFIG
            kubectl apply -n ${params.ENV} -f ${DEPLOYMENT_YAML}
            kubectl apply -n ${params.ENV} -f ${SERVICE_YAML}
          """
        }
      }
    }
  }
}

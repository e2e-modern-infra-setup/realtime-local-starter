pipeline {
  agent { label 'built-in' }
  environment {
    REGISTRY = "registry.platform.svc.cluster.local:5000"
    TAG = "${env.BUILD_NUMBER}"
  }
  stages {
    stage('Build images') {
      steps {
        sh '''
docker build -t ${REGISTRY}/realtime/api:${TAG} apps/api
docker push ${REGISTRY}/realtime/api:${TAG}
'''
      }
    }
    stage('Deploy to dev') {
      steps {
        sh '''
sed -i'' -e "s#newTag: .*#newTag: ${TAG}#g" gitops/k8s/overlays/dev/api/kustomization.yaml
kubectl -n apps-dev apply -k gitops/k8s/overlays/dev
'''
      }
    }
  }
}

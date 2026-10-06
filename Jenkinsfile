pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t myapp:latest .'
      }
    }
    stage('Deploy') {
      steps {
        sh '''
          docker save myapp:latest | docker exec -i minikube sh -c 'if command -v docker >/dev/null 2>&1; then docker load; else ctr -n k8s.io images import -; fi'
          kubectl apply -f deployment.yaml
          kubectl rollout restart deployment/myapp
          kubectl rollout status deployment/myapp --timeout=120s
        '''
      }
    }
  }
}

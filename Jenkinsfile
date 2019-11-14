node {
    def registry = 'sagar437/kubernetes-ansible-aws'
    stage('Checkout git repo') {
      echo 'Git Checkout'
      checkout scm
    }
    stage('Checking environment') {
      echo 'Checking environment...'
      sh 'git --version'
      echo "Branch: ${env.BRANCH_NAME}"
      sh 'docker -v'
      sh 'aws --version'
      sh 'kubectl version'
    }
    stage("Linting") {
      echo 'Linting...'
      sh 'docker run --rm -i hadolint/hadolint < Docker/Dockerfile'
    }
    stage('Build Docker image') {
	    echo 'Building Docker image.'
		sh "cat ~/docker_password.txt | docker login --username sagar437 --password-stdin"
	     	sh "docker build -t ${registry} Docker/."
	     	sh "docker tag ${registry} ${registry}"
	     	sh "docker push ${registry} -t latest_new"
    }
    stage('Deploying') {
      echo 'Deploying to AWS...'
      dir ('./') {
        withAWS(credentials: 'aws-jenkins', region: 'us-west-2') {
            sh "aws eks --region us-west-2 update-kubeconfig --name EKSCluster-UWR9ZWz9MbUw"
            sh "sudo kubectl apply -f eks/aws-auth-cm.yaml"
            sh "sudo kubectl set image deployments/flask-app flask-app=${registry}:latest_new"
            sh "sudo kubectl apply -f deploy.yml"
            sh "sudo kubectl apply -f flask-service.yml"
            sh "sudo kubectl get nodes"
            sh "sudo kubectl get pods"
            sh "sudo kubectl get services -o wide"
        }
      }
    }
    stage("Clean up docker") {
      echo 'Cleaning up docker images'
      sh "docker system prune -f"
    }
}

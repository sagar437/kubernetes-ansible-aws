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
    }
    stage("Linting") {
      echo 'Linting...'
      sh 'docker run --rm -i hadolint/hadolint < Docker/Dockerfile'
    }
    stage('Build Docker image') {
	    echo 'Building Docker image.'
      withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
	     	sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
	     	sh "docker build -t ${registry} Docker/."
	     	sh "docker tag ${registry} ${registry}"
	     	sh "docker push ${registry}"
      }
    }
    stage('Deploying') {
      echo 'Deploying to AWS...'
      dir ('./') {
        withAWS(credentials: 'aws-jenkins', region: 'us-west-2') {
            sh "aws eks --region us-west-2 update-kubeconfig --name EKSCluster-UWR9ZWz9MbUw"
            sh "kubectl apply -f eks/aws-auth-cm.yaml"
            sh "kubectl set image deploy/flask-app flask-app=${registry}:latest"
            sh "kubectl apply -f deploy.yml"
            sh "kubectl get nodes"
            sh "kubectl get pods"
            sh "aws cloudformation update-stack --stack-name nodes --template-body file://eks/nodes/nodes.yml --parameters file://aws/nodes/parameters.json --capabilities CAPABILITY_IAM"
        }
      }
    }
    stage("Cleaning up") {
      echo 'Cleaning up...'
      sh "docker system prune"
    }
}

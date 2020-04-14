node{
  def Namespace = "default"
  def ImageName = "rajcena3535/sayarapp"
  def Creds     = "dockerhub"
  try{
  stage('Checkout'){
      git 'https://github.com/rajcdlmec35/CI-CD-K8s.git'
      sh "git rev-parse --short HEAD > .git/commit-id"
      imageTag= readFile('.git/commit-id').trim()



  }


  stage('RUN Unit Tests'){
      sh "npm install"
      sh "npm test"
  }
  stage('Docker Build, Push'){
    withDockerRegistry([credentialsId: "${Creds}", url: 'https://index.docker.io/v1/']) {
      sh "docker build -t ${ImageName}:${imageTag} ."
      sh "docker push ${ImageName}"
        }

    }
    stage('Deploy on K8s'){

     //sh "ansible-playbook deploy.yml  --user=jenkins --extra-vars ImageName=${ImageName} --extra-vars imageTag=${imageTag} --extra-vars Namespace=${Namespace} -vvv"
      sh "echo 'Helm Install'"
      sh "helm install --name=sayar-${Namespace}  --namespace=${Namespace} ansible/sayarapp --set image.repository=${ImageName} --set image.tag=${imageTag} --set namespace=${Namespace}"
    }
     } catch (err) {
      currentBuild.result = 'FAILURE'
    }
}

#!groovy
def rev 
node('jenkins') {
  stage('Checkout') {
    checkout scm
    echo "$BUILD_NUMBER"
    rev  = $BUILD_NUMBER
  }

  stage('Syntax') {
    echo "${rev}: Syntax and Lint testing"
    sh 'ansible-lint ansible/playbooks/*.yml  ansible/playbooks/*.yaml'
    sh 'ansible-lint ansible/'
    sh "true"
  }

  stage('Build') {
    echo "${rev}: Building vSRX"
    sh "cd ansible;  ansible-playbook playbooks/build_vsrx.yml -e rev=${rev}"
    echo "${rev}: Uploading image"
    sh "sleep 60"
  }

  stage('Deploy') {
    echo "${rev}: Deploying vSRX"
    sh "cd ansible;  ansible-playbook playbooks/deploy_vsrx.yml -e rev=${rev}"
  }
 
  stage('Test') {
    echo  "${rev}: Testing"
    sh "true"
  }

 stage('Approve') {
    echo  "${rev}: Waiting for approval for change"
    input "Do you want to upload the image to pre-prod/prod environment?"
    sh "true"
  }

  stage('Publish') {
    echo "${rev}: Publishing image for community usage"
    echo "Currently this stage is empty"
    sh "true"
  }
}
 

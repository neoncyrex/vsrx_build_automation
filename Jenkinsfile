#!groovy
def rev 
node('jenkins') {
  stage('Checkout') {
    checkout scm
    rev  = "$BUILD_NUMBER"
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
    sh "sleep 120"
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
    sh "cd ansible;  ansible-playbook playbooks/publish_vsrx.yml -e rev=${rev}"
    echo "Pushing changes to permanent git repository"
    def repo = 'git@github.com:neoncyrex/example.git'
    publish_repo_to_subdirectory(repo,'vsrx_build_automation','jenkins', rev)
    sh "true"
  }
}

def publish_repo_to_subdirectory(destrepo,subdirectory,branch,buildnumber) {
   def repodir = getrepodir(destrepo)
   sh "mkdir -p ../builds/build${buildnumber}"
   sh "cd       ../builds/build${buildnumber}; git clone ${destrepo}"
   sh "cd       ../builds/build${buildnumber}/${repodir}; git checkout -b ${branch}"
   sh "mkdir -p ../builds/build${buildnumber}/${repodir}/${subdirectory}"
   sh "cp -r *  ../builds/build${buildnumber}/${repodir}/${subdirectory}"
   sh "cd       ../builds/build${buildnumber}/${repodir}; git add .; git commit -m 'Jenkins build $buildnumber';git push origin ${branch}"
   sh "rm -rf ../builds/build${buildnumber}/${repodir}"
}

def getrepodir(repo) {
   def matcher = repo =~ '/([^/]+).git'
   matcher ? matcher[0][1] : null
} 



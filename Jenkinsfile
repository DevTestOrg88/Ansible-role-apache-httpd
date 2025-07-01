pipeline {
    agent any
triggers {
  githubPush()
}

stages {
  stage('checkoutAnsibleCode') {
    steps {
      // One or more steps need to be included within the steps block.
      git branch: "master", url: 'https://github.com/DevTestOrg88/Ansible-role-apache-httpd.git'
    }
  }
// click “Pipeline Syntax” in the left sidebar - dropdown menu “Sample Step”, choose:withCredentials: Bind credentials to variables
  stage('triggerAnsible') {
    steps {
	withCredentials([usernamePassword(credentialsId: 'ansible_user', passwordVariable: 'PASSWD', usernameVariable: 'USER')]) 
	{
     sh '''sudo ansible-playbook -i apache/tests/inventory apache/tests/test.yml -e "ansible_user=$USER ansible_ssh_pass=$PASSWD"'''
}
}
}
}
// To ensure notifications (like email) run regardless of earlier failures
    post {
            always {
                emailext (
                    attachLog: true,
                    body: 'Check console output at $BUILD_URL to view the results.',
                    subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS!',
                    to: 'rajdeeprm88@gmail.com'
                )
            }
        }
}

pipeline {
   agent none
   stages {
      stage('Build') {
         agent {
            docker {
               image 'blang/latex:ubuntu'
            }
         }
         steps {
             sh 'xelatex sample.tex'
         }
      }
      stage('Publish') {
        steps {
          ansiblePlaybook credentialsId: 'desktop_staffan', disableHostKeyChecking: true, installation: 'Ansible', inventory: 'publish.inv', playbook: 'publish.yaml'
        }
      }
   }
}

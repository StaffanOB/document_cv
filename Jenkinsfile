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
   }
}

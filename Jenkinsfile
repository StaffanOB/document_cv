pipeline {
    agent none
        stages {
            stage('get git tag') {
                agent {
                    docker {
                        image 'blang/latex:ubuntu'
                    }
                }
                steps {
                    script {
                        //latestTag = sh(returnStdout: true, script: "git tag --sort=-creatordate | head -n 1").trim()
                        echo "Building branch ${BRANCH_NAME} with build number ${BUILD_NUMBER}."
                    }
                }
            }

            stage('Build') {
                agent {
                    docker {
                        image 'blang/latex:ubuntu'
                    }
                }
                steps {
                    echo "================================================================="
                    sh 'xelatex ./doc/main.tex'
                    // sh 'mv ./doc/main.pdf ./doc/main.pdf'
                }
            }

            stage('Store') {
                agent {
                    label "node01"
                }
                steps {
                    echo "================================================================="
                    ansiblePlaybook credentialsId: 'desktop_staffan', disableHostKeyChecking: true, extras: '--extra-vars \'{"branch":"${BRANCH_NAME}","build":"${BUILD_NUMBER}"}\'', installation: 'Ansible', inventory: 'publish.inv', playbook: 'publish.yaml'
                }
            }
            stage('Publish') {
                agent {
                    label "node01"
                }
                steps {
                    // ansiblePlaybook credentialsId: 'desktop_staffan', disableHostKeyChecking: true, installation: 'Ansible', inventory: 'publish.inv', playbook: 'publish.yaml'
                    echo "Publish document"
                }
            }

        }
}


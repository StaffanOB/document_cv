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
                        latestTag = sh(returnStdout:  true, script: "git tag --sort=-creatordate | head -n 1").trim()
                        env.BUILD_VERSION = latestTag
                        echo "================================================================="
                        echo "Building version ${BUILD_VERSION} with build number ${BUILD_NUMBER}."
                        echo latestTag
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
                    ansiblePlaybook credentialsId: 'desktop_staffan', disableHostKeyChecking: true, extras: '--extra-vars \'{"version":"${BUILD_VERSION}","build":"${BUILD_NUMBER}"}\'', installation: 'Ansible', inventory: 'publish.inv', playbook: 'publish.yaml'
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


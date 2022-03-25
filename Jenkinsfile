pipeline {
    agent none
        stages {
            stage('get git tag') {
                stage('Build') {
                    agent {
                        docker {
                            image 'blang/latex:ubuntu'
                        }
                    }
                    steps {
                        script {
                            latestTag = sh(returnStdout:  true, script: "git tag --sort=-creatordate | head -n 1").trim()
                                env.BUILD_VERSION = latestTag
                                echo "env-BUILD_VERSION"
                                echo "${env.BUILD_VERSION}"
                        }
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
                    sh 'xelatex sample.tex -job-name=sample-${env.BUILD_VERSION}.pdf'
                }
            }
            stage('Store') {
                agent {
                    label "node01"
                }
                steps {
                    ansiblePlaybook credentialsId: 'desktop_staffan', disableHostKeyChecking: true, installation: 'Ansible', inventory: 'publish.inv', playbook: 'publish.yaml'
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

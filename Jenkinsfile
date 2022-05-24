pipeline {
    options {
        // set a timeout of 30 minutes for this pipeline
        timeout(time: 30, unit: 'MINUTES')
    }
    agent {
      node {
        label 'master'
      }
    }

    stages {

        stage('stage 1') {
            steps {
                script {
                    openshift.withCluster() {
                        openshift.withProject() {
                                echo "stage 1: using project: ${openshift.project()} in cluster ${openshift.cluster()}"
                                echo "Hello from Labuser13"                    
                        }
                    }
                }
            }
        }

        stage('stage 2') {
            steps {
                sh 'echo hello from stage 2!'
                script {
                    openshift.withCluster() {
                        openshift.withProject() {
                               sh 'oc create deployment day5deployment --image quay.io/mayank123modi/mayanknginximage' 
                        }
                    }
                }
            }
        }

        stage('manual approval') {
            steps {
                timeout(time: 60, unit: 'MINUTES') {
                    input message: "Move to stage 3?"
                }
            }
        }

        stage('Create service') {
            steps {
                sh 'echo hello from stage 3!. This is the last stage...'
                script {
                    openshift.withCluster() {
                        openshift.withProject() {
                               sh 'oc expose deployment day5deployment --port 80 --type NodePort' 
                        }
                    }
                }
                sh 'echo Deployment exposed'
            }
        }

        stage('Create Route') {
            steps {
                sh 'echo hello from stage 3!. This is the last stage...'
                script {
                    openshift.withCluster() {
                        openshift.withProject() {
                               sh 'oc expose svc day5deployment' 
                        }
                    }
                }
                sh 'echo Deployment exposed'
            }
        }
    }
}

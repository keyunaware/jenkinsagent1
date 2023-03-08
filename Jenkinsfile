pipeline {

  environment {
    PROJECT = "sandbox-io-289003"
    APP_NAME = "jenkinsagent"
    FE_SVC_NAME = "${APP_NAME}-frontend"
    CLUSTER = "jenkins-cd"
    CLUSTER_ZONE = "us-east1-d"
    IMAGE_TAG = "gcr.io/${PROJECT}/${APP_NAME}:${env.BRANCH_NAME}.${env.BUILD_NUMBER}"
    JENKINS_CRED = "${PROJECT}"
  }

  agent {
    kubernetes {
      label 'jenkinsagenttest'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
labels:
  component: ci
spec:
  # Use service account that can deploy to all namespaces
  serviceAccountName: cd-jenkins
  containers:
    - name: nodejs
    image: node:latest
    command:
    - cat
    tty: true
    - name: python
    image: python:latest
    command:
    - cat
    tty: true
    - name: mysql
    image: mysql:latest
    command:
    - cat
    tty: true
    “”"
}
  }
  stages {
    stage('Compilation') {
       steps {
        container('nodejs') {
          sh "node -v"
          }
        }
      }
    }
    stage('') {
      steps {
        container('python') {
             sh "ctime()${PROJECT}"
         }
        }
      }
    stage('') {
      steps {
        container('terraform') {
           sh "terraform show"
        }
      }
    }
  }
}
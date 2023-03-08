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
      label 'sample-app'
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
  - name:  maven
    image:  maven
    imagePullPolicy: IfNotPresent
    command:
    - cat
    tty: true
  - name: gcloud
    image: gcr.io/cloud-builders/gcloud
    imagePullPolicy: IfNotPresent
    command:
    - cat
    tty: true
  - name: kubectl
    image: gcr.io/cloud-builders/kubectl
    imagePullPolicy: IfNotPresent
    command:
    - cat
    tty: true
"""
}
  }
   stages {
    stage('Compilation') {
       steps {
        container('maven') {
          sh "mvn clean install -DskipTests"
          }
        }
      }
    stage('') {
      steps {
        container('gcloud') {
             sh "PYTHONUNBUFFERED=1 gcloud builds submit -t ${IMAGE_TAG} ."
         }
        }
      }
    stage('') {
      steps {
        container('kubectl') {
           sh("echo http://`kubectl --namespace=production get service/${FE_SVC_NAME} -o jsonpath='{.status.loadBalancer.ingress[0].ip}'` > ${FE_SVC_NAME}")
        }
      }
    }
   }
}
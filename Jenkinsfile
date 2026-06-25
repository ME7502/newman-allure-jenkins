pipeline {
  agent any
  stages {
    stage('install deps') { 
        steps{
            sh 'npm install'
        }
         }
    stage('Execute') {
      agent { docker { image 'node:lts-alpine3.24' } }
      steps {
        sh 'npx newman run collection.json -e env.json --reporters cli,allure --reporter-allure-export allure-results'
        stash includes: 'allure-results/**', name: 'allure-results'
      }
    }
  }
  post {
    always {
      unstash 'allure-results'
      allure includeProperties: false, jdk: '', results: [[path: 'allure-results']]
    }
  }
}
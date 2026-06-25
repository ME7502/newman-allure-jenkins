pipeline {
    agent any 

    stages {

        stage('clean allure results') {
            steps {
                sh '''
                    echo "Suppression du cache Allure..."
                    rm -rf allure-results
                    mkdir -p allure-results
                    echo "Dossier allure-results nettoyé avec succès"
                '''
            }
        }

        stage('install deps') {
            agent { docker { image 'node:lts-alpine3.24' } }  // 2. Docker scoped to stage only
            steps {
                sh 'npm install'
                sh 'npx newman run collection.json -e env.json --reporters cli,allure --reporter-allure-export allure-results'
                stash name: 'allure-results', includes: 'allure-results/**'
            }
        }
    }

    post {
        always {
            script {
                unstash 'allure-results'
                archiveArtifacts artifacts: 'allure-results/**'
                allure includeProperties: false,
                       jdk: '',
                       results: [[path: 'allure-results']]
            }
        }
    }
}
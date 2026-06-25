pipeline {
    agent {
        docker{image 'postman/newman:latest'}
    }

    stages{
                stage('install deps'){
                    steps{
                        sh 'npm ci'
                        sh 'npm install allure'
                    }
                }

                stage('clean allure results'){
                    
                    steps{
                        sh '''
                            echo "Suppression du cache Allure..."
                            rm -rf allure-results
                            mkdir -p allure-results
                            echo "Dossier allure-results nettoyé avec succès"
                        '''
                    }
                }

                stage('Execute'){
                    sh" npx newman run collection.json  -e env.json  --reporters cli,allure  --reporter-allure-export output/allure-results"
                    stash name: 'allure-results', includes: 'allure-results/*'
                    }
    }

    post{
        always{
            script{
                    unstash 'allure-results'
                    archiveArtifacts 'allure-results/*'
                    allure includeProperties: false,
                           jdk: '',
                           results: [[path: 'allure-results/']]
            }
        }
    }
}

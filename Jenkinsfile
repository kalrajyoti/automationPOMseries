pipeline{

    agent any

    stages{

        stage('Start Grid'){
            steps{
                sh "docker-compose -f grid.yaml up -d"
            }
        }

        stage('Run Test'){
            steps{
                sh "docker-compose -f test-suites.yaml up"
            }
        }

    }
       stage('Publish Allure Reports') {
                       steps {
                            script {
                                allure([
                                    includeProperties: false,
                                    jdk: '',
                                    properties: [],
                                    reportBuildPolicy: 'ALWAYS',
                                    results: [[path: '/allure-results']]
                                ])
                            }
                        }
                    }

    post {
        always {
            sh "docker-compose -f grid.yaml down"
            sh "docker-compose -f test-suites.yaml down"
            archiveArtifacts artifacts: 'output/flight-reservation/emailable-report.html', followSymlinks: false
            archiveArtifacts artifacts: 'output/vendor-portal/emailable-report.html', followSymlinks: false
//             archiveArtifacts artifacts: '/allure-results:/home/selenium-docker/allure-results', followSymlinks: false
//             archiveArtifacts artifacts: '/allure-results:/home/selenium-docker/allure-results', followSymlinks: false
        }
    }

}
pipeline{

    agent any
  parameters {
        choice choices: ['chrome', 'firefox'], description: 'Select the browser', name: 'BROWSER'
    }
    stages{

        stage('Start Grid'){
            steps{
                sh "docker-compose -f grid.yaml up  --scale ${params.BROWSER}=2 -d"
            }
        }

        stage('Run Test'){
            steps{
                sh "docker-compose -f test-suites.yaml up"
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
                    }

    post {
        always {
            sh "docker-compose -f grid.yaml down"
            sh "docker-compose -f test-suites.yaml down"

            archiveArtifacts artifacts: 'allure-results/*'
        }
    }

}
pipeline{

agent any

parameters {
  choice choices: ['chrome', 'firefox'], description: 'Select the browser', name: 'BROWSER'
}


stages{
    stage('Start Grid'){
        steps{
            bat "docker-compose -f grid.yaml up --scale ${params.BROWSER}=2 -d"

        }
    }

    stage('Run test'){
        steps{
            bat "docker-compose -f test-suites.yaml up --pull=always"
        }
    }
}

post {
    always {
        bat "docker-compose -f grid.yaml down"
        bat "docker-compose -f test-suites.yaml down"
        archiveArtifacts artifacts: 'flight-reservation/emailable-report.html', followSymlinks: false
        archiveArtifacts artifacts: 'vendor-portal/emailable-report.html', followSymlinks: false
    }
}

}
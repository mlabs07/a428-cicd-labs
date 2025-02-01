node {
    docker.image('node:16-buster-slim').inside("-p 3000:3000") {
        // build, checkout latest code
        stage('Build') {
            checkout scm
            sh 'npm install'
        }
        // test
        stage('Test') {
            sh './jenkins/scripts/test.sh'
        }
        // manual approval
        stage('Manual Approval'){
            input message: 'Lanjut ke tahap Deploy? (Klik "Proceed untuk lanjutkan")'
        }
        // deploy
        stage('Deploy') {
            sh './jenkins/scripts/deliver.sh'
            input message: 'Finished using the website? (Click "Proceed" to continue)'
            sh './jenkins/scripts/kill.sh'

            sleep 60
            echo 'Deploy success'
        }
    }
}
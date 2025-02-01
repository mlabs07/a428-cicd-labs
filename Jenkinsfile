node {
    docker.image('node:16-buster-slim').inside("-p 3000:3000") {
        // build
        stage('Build') {
            checkout scm
            sh 'npm install'
        }
        // test
        stage('Test') {
            sh './jenkins/scripts/test.sh'
        }
        // deploy
        stage('Deploy') {
            sh './jenkins/scripts/deliver.sh'
            input message: 'Finished using the website? (Click "Proceed" to continue)'
            sh './jenkins/scripts/kill.sh'
        }
    }
}
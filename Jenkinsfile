node {
    docker.image('node:16-buster-slim').inside("-p 3000:3000") {
        // build
        stage('Build') {
            sh 'npm install'
        }
        // test
        stage('Test') {
            sh './jenkins/scripts/test.sh'
        }
    }
}
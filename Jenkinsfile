node {
    docker.image('node:18-buster-slim').inside("-p 3000:3000 --user root") {
        // build, checkout latest code
        stage('Build') {
            checkout scm
            withEnv(['NODE_OPTIONS=--openssl-legacy-provider']) {
                sh 'npm install'
                sh 'npm run build'
                sh 'npm install -g vercel'
            }
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
            withCredentials([string(credentialsId: 'vercel_token', variable: 'VERCEL_TOKEN')]) {
                sh 'vercel --token=$VERCEL_TOKEN --prod --confirm'
            }

            sleep 60
            echo 'Deploy success'
        }
    }
}
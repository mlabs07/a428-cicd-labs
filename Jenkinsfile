node {
    docker.image('node:22-buster').inside("-p 3000:3000 --user root") {
        // build, checkout latest code
        stage('Build') {
            checkout scm
            
            sh 'npm install'
            sh 'npm install -g vercel'
        }
        // test
        stage('Test') {
            sh './jenkins/scripts/test.sh'
        }
        // manual approval
        stage('Manual Approval'){
            sh './jenkins/scripts/deliver.sh'
            input message: 'Lanjut ke tahap Deploy? (Klik "Proceed untuk lanjutkan")'
            sh './jenkins/scripts/kill.sh'
        }
        // deploy
        stage('Deploy') {
            withEnv(['NODE_OPTIONS=--openssl-legacy-provider']) {
                withCredentials([string(credentialsId: 'vercel_token', variable: 'VERCEL_TOKEN')]) {
                    sh 'vercel --token=$VERCEL_TOKEN --prod --confirm'
                }
            }

            sleep 60
            echo 'Deploy success'
        }
    }
}
node {
    docker.image('node:18-buster-slim').inside("-p 3000:3000") {
        // build, checkout latest code
        stage('Build') {
            checkout scm
            // sh 'sudo chown -R root:node /usr/local/lib/node_modules'
            // sh 'npm install'
            // sh 'npm install -g vercel --unsafe-perm=true'

            sh 'npm config set prefix /home/node/.npm-global'
            sh 'mkdir -p /home/node/.npm-global/bin'
            sh 'export PATH=/home/node/.npm-global/bin:$PATH'
            
            // Install dependencies
            sh 'npm install'
            sh 'npm install -g vercel --unsafe-perm=true'

            sh 'vercel --version'

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
            withCredentials([string(credentialsId: 'vercel_token', variable: 'VERCEL_TOKEN')]) {
                sh 'vercel --token=$VERCEL_TOKEN --prod --confirm'
            }

            sleep 60
            echo 'Deploy success'
        }
    }
}
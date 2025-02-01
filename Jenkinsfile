node {
    docker.image('node:16-buster').inside("-p 3000:3000") {
        // build, checkout latest code
        stage('Build') {
            checkout scm
            sh 'apt-get update && apt-get install -y zip'
            sh 'npm install'
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
            
            sh 'zip -r build.zip build'
            withCredentials([string(credentialsId: 'vercel_token', variable: 'VERCEL_TOKEN')]) {
                sh '''
                curl -X POST https://api.vercel.com/v13/deployments \
                    -H "Authorization: Bearer $VERCEL_TOKEN" \
                    -F "files=build.zip" \
                    -F "name=react-app-zulqifli" \
                    -F "target=production"
                '''
            }

            sleep 60
            echo 'Deploy success'
        }
    }
}
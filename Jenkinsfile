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
            sh './jenkins/scripts/deliver.sh'
            input message: 'Lanjut ke tahap Deploy? (Klik "Proceed untuk lanjutkan")'
            sh './jenkins/scripts/kill.sh'
        }
        // deploy
        stage('Deploy') {
            withCredentials([string(credentialsId: 'vercel_token', variable: 'VERCEL_TOKEN')]) {
                sh '''
                wget --method=POST \
                    --header="Authorization: Bearer $VERCEL_TOKEN" \
                    --body-file=<(echo -e "files=@./build/*\nname=react-app-zulqifli\ntarget=production") \
                    https://api.vercel.com/v13/deployments
                '''
            }

            sleep 60
            echo 'Deploy success'
        }
    }
}
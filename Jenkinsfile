pipeline {
    agent any
    // notify via telegram
    environment {
        TELEGRAM_TOKEN = credentials('TOKEN') // token id
        CHAT_ID    = credentials('Chat_ID') // chat id
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                sh 'ssh -o StrictHostKeyChecking=no "username"@"ip server" "cd ~/app/development/api;\
                git pull;\
                cd ..;\
                docker-compose up -d --build;\
                "'
            }
        }
    }
      post{
    success{

        sh ''' 
        curl -s -X POST https://api.telegram.org/bot${TELEGRAM_TOKEN}/sendMessage -d chat_id=${CHAT_ID} -d parse_mode="HTML" -d text="<b>Stage</b> : API \n<b>Status</b> : <i>Success</i> \n<b>Version</b>: ${BUILD_NUMBER} <b> \nEnvironment </b> : Development"
        '''
    }
    failure{
        sh ''' 
        curl -s -X POST https://api.telegram.org/bot${TELEGRAM_TOKEN}/sendMessage -d chat_id=${CHAT_ID} -d parse_mode="HTML" -d text="<b>Stage</b> : API \n<b>Status</b> : <i>Failed</i> \n<b>Version</b>: ${BUILD_NUMBER} <b> \nEnvironment </b> : Development"
        '''
    }
    }
}

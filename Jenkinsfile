pipeline {
    agent any
    environment {
        LAUNCH_JOB_NAME = 'Detonado/detonado/develop'
        WAIT_TIMEOUT = '120'
    }
    stages {
        stage('Registart WebHook') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'jenkins-api-credentials', variable: 'SECRET')]) {
                        hook = registerWebhook(token: 'webhook', authToken: SECRET)
                        echo "Waiting for POST to ${hook.url}"
                        build job: env.LAUNCH_JOB_NAME,
                            parameters: [string(name: 'CALLBACK_URL', value: hook.url)],
                            wait: false
                        echo "Waiting for POST to ${hook.url}"
                        data = waitForWebhook hook
                        echo "Callback recibido: ${data}"
                    }
                }
            }
        }
    }
}

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
                    hook = registerWebhook()
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

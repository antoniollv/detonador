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
                    }
                    parallel {
                        stage ('Launch Job') {
                            steps {
                                script {
                                    build job: env.LAUNCH_JOB_NAME,
                                    parameters: [string(name: 'CALLBACK_URL', value: env.CALLBACK_URL)],
                                    wait: false
                                }
                            }
                        }
                        stage('Continuar procesamiento') {
                            steps {
                                echo 'El primer pipeline continúa con otras tareas...'
                                // Simulación de otras tareas
                                sleep time: 3, unit: 'SECONDS'
                            }
                        }
                        stage('Esperar Callback del Segundo Pipeline') {
                            steps {
                                script {
                                    echo "Waiting for POST to ${callbackURL}"
                                    data = waitForWebhook hook
                                    echo "Callback recibido: ${data}"
                                }
                            }
                        }
                    }
                }
            }
        }
    }
}

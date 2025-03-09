pipeline {
    agent any
    environment {
        // Token predefinido para el callback
        WEBHOOK_TOKEN = 'myToken'
        // URL base (según tu configuración de Jenkins) para el endpoint del webhook.
        // Nota: La URL del endpoint dependerá de cómo el plugin Webhook Step exponga el listener.
        WEBHOOK_ENDPOINT = "https://jenkins.moradores.es/webhook"
    }
    stages {
        stage('Lanzar Segundo Pipeline') {
            steps {
                script {
                    echo "Lanzando el segundo pipeline..."
                    // Se dispara el segundo pipeline de forma asíncrona y se le pasa el token
                    build job: "SegundoRepositorio/master", 
                          parameters: [string(name: 'WEBHOOK_TOKEN', value: env.WEBHOOK_TOKEN)],
                          wait: false
                }
            }
        }
        stage('Continuar procesamiento') {
            steps {
                echo "El primer pipeline continúa con otras tareas..."
                // Simulación de otras tareas
                sleep time: 3, unit: 'SECONDS'
            }
        }
        stage('Esperar Callback del Segundo Pipeline') {
            steps {
                script {
                    echo "Esperando callback en el endpoint ${env.WEBHOOK_ENDPOINT}?token=${env.WEBHOOK_TOKEN}"
                    // Aquí se utiliza el step proporcionado por el plugin Webhook Step para esperar el callback.
                    // La sintaxis depende de la versión del plugin; en este ejemplo se asume un step llamado waitForWebhook.
                    def callbackData = waitForWebhook(
                        token: env.WEBHOOK_TOKEN,
                        timeout: 120 // en segundos
                    )
                    echo "Callback recibido: ${callbackData}"
                    // Puedes asignar la información recibida a una variable de entorno o procesarla según necesites.
                    env.CALLBACK_DATA = callbackData
                }
            }
        }
        stage('Mostrar Información del Segundo Pipeline') {
            steps {
                echo "Datos recibidos del segundo pipeline:"
                echo "${env.CALLBACK_DATA}"
            }
        }
    }
}

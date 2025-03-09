// Configuración del trigger para recibir el webhook a través del plugin Generic Webhook Trigger Plugin
properties([
  pipelineTriggers([
    [
      $class: 'GenericTrigger',
      genericVariables: [
         [key: 'HOOK_JOB_NUMBER', value: '$.jobNumber'],
         [key: 'HOOK_REPO',       value: '$.repo'],
         [key: 'HOOK_DURATION',   value: '$.duration'],
         [key: 'HOOK_STATUS',     value: '$.status']
      ],
      causeString: 'Triggered by webhook callback',
      token: 'myToken', // Debe coincidir con el token que usará el segundo pipeline
      printContributedVariables: true,
      printPostContent: true
    ]
  ])
])

pipeline {
    agent any
    environment {
        // Configuración para disparar el segundo repositorio (multibranch)
        // Se debe usar la ruta interna, por ejemplo "SegundoRepositorio/master"
        SECOND_JOB_NAME = "Detonado/detonado/develop"
        // Nota: este job se dispara de forma asíncrona y la información del callback se recibirá en otra ejecución
        WAIT_TIMEOUT   = "120"
    }
    stages {
        stage('Disparar Segundo Pipeline') {
            steps {
                script {
                    echo "Disparando Segundo Pipeline..."
                    // Se dispara el segundo pipeline sin esperar su finalización.
                    build job: env.SECOND_JOB_NAME, wait: false
                    echo "El segundo pipeline se ha disparado; espere a que el callback dispare este job nuevamente con los datos."
                }
            }
        }
        stage('Mostrar información del Callback (Webhook)') {
            steps {
                script {
                    // Si este build fue disparado vía webhook, se tendrán definidos los parámetros HOOK_*
                    if (env.HOOK_JOB_NUMBER) {
                        echo "Información recibida vía webhook:"
                        echo "Job Number: ${env.HOOK_JOB_NUMBER}"
                        echo "Repo: ${env.HOOK_REPO}"
                        echo "Duration: ${env.HOOK_DURATION} seg"
                        echo "Status: ${env.HOOK_STATUS}"
                    } else {
                        echo "Esta ejecución no recibió datos vía webhook."
                    }
                }
            }
        }
    }
}

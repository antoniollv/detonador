// Función para lanzar el job del segundo repositorio y esperar su resultado
def triggerSecondJob(jobName, timeoutSeconds) {
    def resultMap = [:]
    try {
        // Se espera el resultado del job, con un timeout configurado
        def jobResult
        timeout(time: timeoutSeconds, unit: 'SECONDS') {
            // Lanza el job sin propagar error (para poder manejar el estado manualmente)
            jobResult = build job: jobName, wait: true, propagate: false
        }
        // Calcula el tiempo de duración en segundos (duration viene en milisegundos)
        def durationSec = jobResult.duration / 1000
        resultMap.number = jobResult.number
        resultMap.repo = jobName
        resultMap.duration = durationSec
        resultMap.status = jobResult.result
    } catch (err) {
        // Si se excede el timeout o ocurre un error, se marca la build como inestable
        resultMap.number = "N/A"
        resultMap.repo = jobName
        resultMap.duration = "Excede ${timeoutSeconds} seg."
        resultMap.status = "FAILURE"
        currentBuild.result = 'UNSTABLE'
        echo "Error o timeout en job ${jobName}: ${err}"
    }
    // Devuelve una cadena formateada con la información relevante
    return "Job Number: ${resultMap.number}, Repo: ${resultMap.repo}, Duration: ${resultMap.duration} seg, Status: ${resultMap.status}"
}

pipeline {
    agent any
    environment {
        // Configuración para el segundo repositorio (se puede modificar fácilmente)
        SECOND_JOB_NAME = "Detonado/detonado/main"  // Nombre del job a disparar
        WAIT_TIMEOUT   = "120"                     // Tiempo de espera en segundos
    }
    stages {
        stage('Lanzar Segundo Pipeline') {
            steps {
                script {
                    // Se invoca la función para disparar y esperar el segundo pipeline
                    def jobInfo = triggerSecondJob(env.SECOND_JOB_NAME, WAIT_TIMEOUT.toInteger())
                    // Se guarda la información para mostrarla posteriormente
                    env.SECOND_JOB_INFO = jobInfo
                }
            }
        }
        stage('Mostrar información del Segundo Pipeline') {
            steps {
                echo "Información del Segundo Pipeline:"
                echo "${env.SECOND_JOB_INFO}"
            }
        }
    }
}

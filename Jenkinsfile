pipeline {
    agent none
    environment {
        IDEMPIERE_VERSION = "10.0.0"
    }
    stages {
        stage('Set Environment Variables') {
            steps {
                script {
                    echo "JOB_NAME received: ${env.JOB_NAME}"
                    // Definir las variables de entorno basadas en JOB_NAME
                    def PLUGIN_NAME = ''
                    def PLUGIN_NAME2 = ''
                    if (env.JOB_NAME == 'org.globalqss.idempiere.LCO.detailednames') {
                        PLUGIN_NAME = 'org.globalqss.idempiere.LCO.detailednames'
                    } else if (env.JOB_NAME == 'org.globalqss.idempiere.LCO.withholdings') {
                        PLUGIN_NAME2 = 'org.globalqss.idempiere.LCO.withholdings'
                    }
                    // Ahora pluginName estará disponible en todo el script
                    def pluginName = PLUGIN_NAME ?: PLUGIN_NAME2
                    // Determinar el nombre del plugin a compilar
                    if (!pluginName) {
                        error "Plugin name not found"
                    }
                    // Compilar el plugin
                    sh "./plugin-builder build ../${pluginName}"
                    // Archivar el artefacto
                    archiveArtifacts artifacts: "target/${pluginName};singleton:=true-${IDEMPIERE_VERSION}.${BUILD_NUMBER}.jar", fingerprint: true
                    // Eliminar directorios temporales
                    sh "rm -rf target ../${pluginName}/target ../${pluginName}.test/target"
                }
            }
        }
    }
}

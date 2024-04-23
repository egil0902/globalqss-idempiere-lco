pipeline {
    agent none
    environment {
        IDEMPIERE_VERSION = "10.0.0"
        PLUGIN_NAME = ''
        PLUGIN_NAME2 = ''
    }
    stages {
        stage('Set Environment Variables') {
            steps {
                script {
                    // Definir las variables de entorno basadas en JOB_NAME
                    if (env.JOB_NAME == 'org.globalqss.idempiere.LCO.detailednames') {
                        PLUGIN_NAME = 'org.globalqss.idempiere.LCO.detailednames'
                    } else if (env.JOB_NAME == 'org.globalqss.idempiere.LCO.withholdings') {
                        PLUGIN_NAME2 = 'org.globalqss.idempiere.LCO.withholdings'
                    }
                }
            }
        }
        stage('Compile') {
            agent {
                docker {
                    image 'idempiereofficial/idempiere:source-release-10.0'
                    args '-u root:root'               
                  }
            }
            steps {
                dir('target-platform') {
                    git branch: '10', url: 'https://github.com/ingeint/idempiere-target-platform-plugin.git'
                    script {
                        // Determinar el nombre del plugin a compilar
                        def pluginName = PLUGIN_NAME ?: PLUGIN_NAME2
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
}

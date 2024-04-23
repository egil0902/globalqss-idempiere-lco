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
                    echo "JOB_NAME received: ${env.JOB_NAME}"
                    // Definir las variables de entorno basadas en JOB_NAME
                    if (env.JOB_NAME == 'org.globalqss.idempiere.LCO.detailednames/10.0.0') {
                        PLUGIN_NAME = 'org.globalqss.idempiere.LCO.detailednames'
                    } else if (env.JOB_NAME == 'org.globalqss.idempiere.LCO.withholdings/10.0.0') {
                        PLUGIN_NAME = 'org.globalqss.idempiere.LCO.withholdings'
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
                        
                        }
                        sh "./plugin-builder build ../${PLUGIN_NAME}"
                        archiveArtifacts artifacts: "target/${PLUGIN_NAME};singleton:=true-${IDEMPIERE_VERSION}.${BUILD_NUMBER}.jar", fingerprint: true
                        sh "rm -rf target ../${PLUGIN_NAME}/target ../${PLUGIN_NAME}.test/target"
                    }
                }
            }
        }
    }
}

pipeline {
    agent none
    environment {
        PLUGIN_NAME = "org.globalqss.idempiere.LCO.detailednames"
        PLUGIN_NAME2 = "org.globalqss.idempiere.LCO.withholdings"
       
        IDEMPIERE_VERSION = "10.0.0"
    }
    stages {
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
                   
					sh './plugin-builder build ../${PLUGIN_NAME} ../${PLUGIN_NAME2}' 
                    archiveArtifacts artifacts: "target/${PLUGIN_NAME};singleton:=true-${IDEMPIERE_VERSION}.${BUILD_NUMBER}.jar", fingerprint: true
                    archiveArtifacts artifacts: "target/${PLUGIN_NAME};singleton:=true-${IDEMPIERE_VERSION}.${BUILD_NUMBER}.jar", fingerprint: true
                    sh 'rm -rf target ../${PLUGIN_NAME}/target ../${PLUGIN_NAME}.test/target'
                    sh 'rm -rf target ../${PLUGIN_NAME2}/target ../${PLUGIN_NAME}.test/target'
                    
                }
            }
        }
    }
}

pipeline {
    agent any
    environment {
        NEXUS_USER         = credentials('nexus-user')
        NEXUS_PASSWORD     = credentials('nexus-password')
    }
    parameters {
        choice(
            name:'compileTool',
            choices: ['Maven', 'Gradle'],
            description: 'Seleccione herramienta de compilacion'
        )
    }
    stages {
        stage("Pipeline"){
            steps {
                script{
                  switch(params.compileTool)
                    {
                        case 'Maven':
                            def ejecucion = load 'maven.groovy'
                            ejecucion.call()
                        break;
                        case 'Gradle':
                            def ejecucion = load 'gradle.groovy'
                            ejecucion.call()
                        break;
                    }
                }
            }
            post{
				success{
					slackSend color: 'good', message: "[JESUS DONOSO] [${env.BUILD_TAG}] Ejecucion Exitosa", teamDomain: 'jesus-testespacio', tokenCredentialId: 't-slack'
				}
				failure{
					slackSend color: 'danger', message: "[JESUS DONOSO] [${env.BUILD_TAG}] Ejecucion fallida en stage [${env.FAIL_STAGE_NAME}]", teamDomain: 'jesus-testespacio', tokenCredentialId: 't-slack'
				}
			}
        }
    }
}
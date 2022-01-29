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
        }
    }
    post{
        always {
                step {
                    if ( buildResult == "SUCCESS" ) {
                    slackSend (color: 'good', message: "[Jesus Donoso] [${JOB_NAME}] [${BUILD_TAG}] Ejecucion Exitosa")
                    }
                    if( buildResult == "FAILURE" ) {
                        slackSend (color: 'danger', message: "[Jesus Donoso] [${env.JOB_NAME}] [${BUILD_TAG}] Ejecucion fallida en stage [${env.TAREA}]")
                    }
                }
			}
		}
    }
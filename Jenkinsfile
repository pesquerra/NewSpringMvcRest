pipeline {
    agent any
    
    stages {

        stage('inicio') { 
            steps {
                echo "Inicio"
            }
        }
        stage('Contruccion') { 
            steps {
            echo "Contruccion"
                bat 'mvn -B package' 
            }
        }
        stage('Test') { 
            steps {
             echo "Test"
                bat 'mvn clean verify' 
            }
        }
        
        stage('SonarQube analysis') {
            environment {
                //Se configura la conexion mediante el nombre configurado en Jenkins
                SCANNER_HOME = tool 'SonarQube Conection'
            }
            steps {
                withSonarQubeEnv(credentialsId: 'SonarQube', installationName: 'SonarQube') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner \
                    //Se configura el repositorio con las configuraciones de Nexus
                    -Dsonar.projectKey=Grupo5 \
                    -Dsonar.projectName=Grupo5 \
                    -Dsonar.sources=src/ \
                    -Dsonar.java.binaries=target/classes/ \
                    -Dsonar.exclusions=src/test/java/****/*.java \
                    -Dsonar.projectVersion=${BUILD_NUMBER}-${GIT_COMMIT_SHORT}'''
                }
            }
        }
        
 
        
    }
    /*
    post {
        always{
            slackSend( channel: "#grupo5", token: "ffx7Fj80ByIBWTpKm7bj3M2L", color: "good", message: "Prueba Pablo - inicio")
        }
        failure{
            slackSend(channel: "#grupo5", token: "ffx7Fj80ByIBWTpKm7bj3M2L", color: "good", message: ":warning: Prueba Pablo - Hubo uns Falla :warning: ")
        }
        success{
            slackSend(channel: "#grupo5", token: "ffx7Fj80ByIBWTpKm7bj3M2L", color: "good", message: ":smile: Prueba Pablo - Ejecución Exitosa :smile: ")
        }
    }
    */
}

pipeline {
    agent any
    environment {
        //Conexion Nexus
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "localhost:8081/repository/Grupo5"
        NEXUS_REPOSITORY = "Leccion7"
        NEXUS_CREDENTIAL_ID = "Nexus"        
    }
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

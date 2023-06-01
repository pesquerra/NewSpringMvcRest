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

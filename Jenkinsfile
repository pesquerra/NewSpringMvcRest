pipeline {
    agent any
    environment {
        //Conexion Nexus
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "localhost:8081/repository/Leccion7"
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
        
//        stage('SonarQube analysis') {
//            environment {
//                //Se configura la conexion mediante el nombre configurado en Jenkins
//                SCANNER_HOME = tool 'SonarQube Conection'
//            }
//            steps {
//                withSonarQubeEnv(credentialsId: 'SonarQube', installationName: 'SonarQube') {
//                    bat '''$SONARQUBE-HOME/bin/windows-x86-64 \
//                    //Se configura el repositorio con las configuraciones de Nexus
//                    -Dsonar.projectKey=Leccion7 \
//                    -Dsonar.projectName=Leccion7 \
//                    -Dsonar.sources=src/ \
//                    -Dsonar.java.binaries=target/classes/ \
//                    -Dsonar.exclusions=src/test/java/****/*.java \
//                    -Dsonar.projectVersion=${BUILD_NUMBER}-${GIT_COMMIT_SHORT}'''
//                }
//            }
//        }
        
        stage("Publish to Local Nexus Repository Manager") {
            steps {
                script {
                    echo "Iniciando Nexus"
                    pom = readMavenPom file: "pom.xml";
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path;
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo "***  NEXUS - Archivo: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                        nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: artifactPath,
                                type: pom.packaging],
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );
                    } else {
                        error "*** NEXUS - Archivo: ${artifactPath}, no fue encontrado";
                    }
                }
            }
        }

    }
    
    post {
        always{
            slackSend( channel: "#fundamentos-devops", token: "SlackEsquemon", color: "good", message: "Prueba Pablo - inicio")
        }
        failure{
            slackSend(channel: "#fundamentos-devops", token: "SlackEsquemon", color: "good", message: ":warning: Prueba Pablo - Hubo uns Falla :warning: ")
        }
        success{
            slackSend(channel: "#fundamentos-devops", token: "SlackEsquemon", color: "good", message: ":smile: Prueba Pablo - Ejecución Exitosa :smile: ")
        }
    }
    
}

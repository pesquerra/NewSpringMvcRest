pipeline {
    agent any
    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "localhost:8081"
        NEXUS_REPOSITORY = "RepoPrueba5"
        NEXUS_CREDENTIAL_ID = "nexus-admin-password"
    }
    stages {
        stage('Inicio') { 
            steps {
                echo "Este es el inicio"
            }
        }
        stage('Build') { 
            steps {
                sh 'mvn -B package' 
                // sh 'mvn -B -DskipTests clean package' 
            }
        }
        stage('Test') { 
            steps {
                sh 'mvn clean verify' 
            }
        }
        stage('SonarQube analysis') {
            environment {
                SCANNER_HOME = tool 'SonarQubeLocal'
            }
            steps {
                withSonarQubeEnv(credentialsId: 'AccessToSonarQube', installationName: 'SonarQubeLocal') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner \
                    -Dsonar.projectKey=Demo4 \
                    -Dsonar.projectName=Demo4 \
                    -Dsonar.sources=src/ \
                    -Dsonar.java.binaries=target/classes/ \
                    -Dsonar.exclusions=src/test/java/****/*.java \
                    -Dsonar.projectVersion=${BUILD_NUMBER}-${GIT_COMMIT_SHORT}'''
                }
            }
        }
        stage("Publish to Local Nexus Repository Manager") {
            steps {
                script {
                    pom = readMavenPom file: "pom.xml";
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path;
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
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
                        error "*** File: ${artifactPath}, could not be found";
                    }
                }
            }
        }
    }
    post {
        always{
            junit (
                allowEmptyResults: true,
                testResults: '*/test-reports/.xml'
            )   
        }
        success {
            slackSend(channel: "#pruebas-jenkins", token: "Slack-cursodevops2023", color: "good", message: "Exito")
            slackSend(channel: '#pruebas-jenkins', color: '#6EAB00', message: 'Funcionó :smile: :laughing: :wink: :thumbsup: :thumbsdown: :exclamation: :question: :fire: :rocket: :hourglass: :checkered_flag: :coffee: :beer: :pizza: :tada: :star: ', teamDomain: 'cursodevops2023', tokenCredentialId: 'Slack-cursodevops2023', username: 'jenkins') 
        }
        failure{
            slackSend(channel: "#fundamentos-de-devops", token: "slack_webhook token", color: "good", message: "${custom_msg()}")
        }
    }
    def custom_msg() {
        def JENKINS_URL= "localhost:8080"
        def JOB_NAME = env.JOB_NAME
        def BUILD_ID= env.BUILD_ID
        def JENKINS_LOG= " FAILED: Job [${env.JOB_NAME}] Logs path: ${JENKINS_URL}/job/${JOB_NAME}/${BUILD_ID}/consoleText"
        return JENKINS_LOG
    }
}
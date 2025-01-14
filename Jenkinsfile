pipeline {
    agent any
        stages {
          stage('Build') {
            steps {
               bat 'start cmd /c mvn -B package'
            }
        }
        stage('SonarQube analysis') {
             environment {
                SCANNER_HOME = tool 'SonarQube'
            }
            steps {
              withSonarQubeEnv(credentialsId: 'sonar', installationName: 'SonarQube') {
              bat '''start cmd /c $SCANNER_HOME/bin/sonar-scanner \
              start cmd /c -Dsonar.projectKey=projectKey \
            start cmd /c -Dsonar.projectName=projectName \
               start cmd /c -Dsonar.sources=src/ \
               start cmd /c -Dsonar.java.binaries=target/classes/ \
            start cmd /c -Dsonar.exclusions=src/test/java/****/*.java \
             start cmd /c -Dsonar.projectVersion=${BUILD_NUMBER}-${GIT_COMMIT_SHORT}'''
             }
            }
        }
        stage("Publish to Nexus Repository Manager") {
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
                            nexusVersion: "nexus3",
                            protocol: "http",
                            nexusUrl: "localhost:8081",
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: "actividad-grupal",
                            credentialsId: "nexus",
                            artifacts: [
                                [artifactId: pom.artifactId,
                                        classifier: '',
                                        file: artifactPath,
                                        type: pom.packaging]
                            ]
                        );
                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
                }
            }
            }
     }
    post{
        failure{
            slackSend( channel: "#actividad-grupal-modulo-3", color: "good", message: "${custom_msg()}")
        }
    }
}
   def custom_msg()
        {
        def JENKINS_URL= "localhost:8080"
        def JOB_NAME = env.JOB_NAME
        def BUILD_ID= env.BUILD_ID
        def JENKINS_LOG= " FAILED: Job [${env.JOB_NAME}] Logs path: ${JENKINS_URL}/job/${JOB_NAME}/${BUILD_ID}/consoleText"
        return JENKINS_LOG
    }

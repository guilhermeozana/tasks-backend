pipeline {
    agent any
    stages {
        stage ('Build Backend') {
            steps {
                bat 'mvn clean package -DskipTests=true'
            }
        }

        stage ('Unit Tests') {
            steps {
                bat 'mvn clean test verify'
            }
        }

        stage ('Sonar Analysis') {
            environment {
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps {
                withSonarQubeEnv('SONAR_LOCAL') {
                    bat "\"${scannerHome}/bin/sonar-scanner\" -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=sqp_dfdb9e452696a5c61932d7fd8ecb26438841fd56 -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**Application.java,**/src/main/java/br/ce/wcaquino/taskbackend/controller/**,**/src/main/java/br/ce/wcaquino/taskbackend/utils/ValidationException.java"
                }
            }
        }

        stage ('Quality Gate') {
            steps {
                def qualitygate = waitForQualityGate()
                if (qualitygate.status != "OK") {
                   error "Pipeline aborted due to quality gate coverage failure: ${qualitygate.status}"
                }
            }

        }
    }
}



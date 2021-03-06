pipeline {
    agent any

    stages{
        stage('Build Backend') {
            steps {
                sh 'mvn clean package -DskipTests=true'
            }
        }
        stage('Unit Tests') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Sonar Analysis') {
            environment {
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps {
                withSonarQubeEnv('SONAR_LOCAL') {
                    sh "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://jenkins:9000 -Dsonar.login=dd7414e2f3e34404c6b495f6fcb100055b2681b5 -Dsonar.java.binaries=target"
                }                
            }
        }
    }
}

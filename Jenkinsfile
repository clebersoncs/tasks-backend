pipeline {
    agent any

    stages{
        /*---------------------------------------------------- BUILD BACKEND */
        stage('Build Backend') {
            steps {
                sh 'mvn clean package -DskipTests=true'
            }
        }

        /*-------------------------------------------------------- UNIT TEST */
        stage('Unit Tests') {
            steps {
                sh 'mvn test'
            }
        }

        /*--------------------------------------------------- SONAR ANALYSIS */
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
        
        /*------------------------------------------------------ QUALITY GATE */
        // stage('Quality Gate') {
        //     steps {
        //         sleep(5)
        //         timeout(time: 1, unit: 'MINUTES') {
        //             waitForQualityGate abortPipeline: true
        //         }
        //     }
        // }

        /*---------------------------------------------------- DEPLOY BACKEND */
        stage('Deploy Backend') {
            steps {
                deploy adapters: [tomcat8(credentialsId: 'TomcatLogin', path: '', url: 'http://jenkins:8001/')], contextPath: 'tasks-backend', onFailure: false, war: 'target/tasks-backend.war'
            }
        }
        
    }
}

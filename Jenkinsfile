pipeline {
    agent any
    
    stages {
        stage('Git check out code') {
            steps {
                Git ''
            }
        }

        stage ('Buliding code') {
            steps {
                sh 'mvn clean install -DskipTests'
            }
        }

        stage('unit testing code') {
            steps {
                sh ' mvn test'
            }
        }

        stage(' SonarQude analysis') {
            environment {
                scannerHome = tool 'sonar-scanner'
            }
            steps {
                script {
                    withSonarQubeEnv('sonar-scanner') {
                        sh '${scannerHome}/bin/sonar-scanner'
                    }
                }
            }
        }

        stage('SonarQude code validation') {
            steps {
                script {
                    timeout(time: 1, unit: 'HOURS') {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }
                    }
                }
            }
        }
    }
}
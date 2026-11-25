pipeline {
    agent any

    tools {
        maven 'maven_devops'
    }

    triggers {
        githubPush()
    }

    environment {
        POM = "pom.xml"
    }

    stages {
        stage('Descarga') {
            steps {
                git url: 'https://github.com/TuPapiJorgito/ActividadesNetbeans.git', branch: 'main'
            }
        }
        stage('Compilacion') {
            steps {
                script {
                    if (isUnix()) { sh 'mvn -f $POM -B package' }
                    else          { bat 'mvn -f %POM% -B package' }
                }
            }
        }
        stage('Prueba') {
            steps {
                script {
                    if (isUnix()) { sh 'mvn -f $POM test' }
                    else          { bat 'mvn -f %POM% test' }
                }
            }
            post {
                always {
                    junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Empaquetado') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }
}

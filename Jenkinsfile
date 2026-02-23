pipeline {
    agent any
        tools {
        maven 'Maven'
        }
    stages {
        stage('Compilación') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('SonarQube análisis') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage('Nexus Upload') {
            steps {
                echo 'Subiendo archivo .jar a Nexus...'
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: 'nexus:8081',
                    groupId: 'com.ismail',
                    version: '0.0.1-SNAPSHOT',
                    repository: 'maven-snapshots',
                    credentialsId: 'nexus-credential',
                    artifacts: [
                        [artifactId: 'issuetracking', file: 'target/issuetracking-0.0.1-SNAPSHOT.jar', type: 'jar']
                    ]
                )
            }
        }
        stage('Docker Build') {
            steps {
                echo 'Creando imagen de Docker...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Desplegando la aplicación...'
            }
        }
    }
}

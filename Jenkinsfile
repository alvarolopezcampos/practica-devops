pipeline {
    agent any
    stages {
        stage('Compile') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                echo 'Analizando código con SonarQube...'
            }
        }
        stage('Nexus Upload') {
            steps {
                echo 'Subiendo archivo .jar a Nexus...'
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

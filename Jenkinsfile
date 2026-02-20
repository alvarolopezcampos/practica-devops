pipeline {
    agent any
    
    tools {
        maven 'Maven' 
    }
    stages {
        stage('Compile') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('SonarQube Analysis') {
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
                echo 'Subiendo el archivo .jar a Nexus...'
                // Sustituye 'nexus-credentials' por el ID exacto que le pusiste a tu credencial en Jenkins
                withCredentials([usernamePassword(credentialsId: 'nexus-credentials', passwordVariable: 'NEXUS_PASS', usernameVariable: 'NEXUS_USER')]) {
                    sh "mvn deploy -DskipTests -Dnexus.url=http://nexus:8081 -Dusername=${NEXUS_USER} -Dpassword=${NEXUS_PASS}"
                }
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


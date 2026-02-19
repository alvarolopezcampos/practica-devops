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
                withCredentials([usernamePassword(credentialsId: 'nexus-credentials', passwordVariable: 'NEXUS_PWD', usernameVariable: 'NEXUS_USR')]) {
                    sh '''
                    echo "<settings><servers><server><id>nexus-snapshots</id><username>${NEXUS_USR}</username><password>${NEXUS_PWD}</password></server></servers></settings>" > settings.xml
                    mvn deploy -s settings.xml -DskipTests
                    '''
                }
            }
        }
        stage('Docker Build') {
            steps {
                sh 'docker build -t my-app:latest .'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Desplegando la aplicación...'
            }
        }
    }
}

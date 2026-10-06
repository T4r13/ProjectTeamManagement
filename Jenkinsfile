pipeline {
    agent any

    options {
        timestamps()
        disableConcurrentBuilds()
    }

    triggers {
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/T4r13/ProjectTeamManagement.git'
            }
        }

        stage('Verification environnement') {
            steps {
                sh 'java -version'
                sh 'mvn -v'
            }
        }

        stage('Tests unitaires') {
            steps {
                dir('backend/backend') {
                    sh 'mvn clean test'
                }
            }
            post {
                always {
                    junit allowEmptyResults: true, testResults: 'backend/backend/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Build') {
            steps {
                dir('backend/backend') {
                    sh 'mvn package -DskipTests'
                }
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: 'backend/backend/target/*.jar', fingerprint: true
            echo 'Build réussi : livrable créé dans backend/backend/target/'
        }
        failure {
            echo 'Le build a échoué'
        }
    }
}
pipeline {
    agent any

    stages {
        stage('Bundle') {
            steps {
                sh './gradlew bundleRelease'
            }
        }
        stage('Test') {
            steps {
                sh './gradlew test'
            }
        }
        stage('Build') {
            steps {
                sh './gradlew assembleDebug'
            }
        }
post {
    success {
        archiveArtifacts artifacts: 'app/build/outputs/apk/release/*.apk'
    }
}
}

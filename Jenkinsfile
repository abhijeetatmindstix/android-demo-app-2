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
        stage('Archive') {
            steps {
                archiveArtifacts artifacts: '**/*.apk' // command to archive the artifacts
            }
        }        

 }
}







// post {
//     success {
//         archiveArtifacts artifacts: 'app/build/outputs/apk/release/*.apk'
//     }

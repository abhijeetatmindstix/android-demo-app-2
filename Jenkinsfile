pipeline {
    agent any

    stages {
        stage('Bundle') {
            steps {
                sh './gradlew bundleRelease'
            }
        }
    }
}

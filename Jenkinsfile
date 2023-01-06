 pipeline {
    agent any
    stages {
        stage("Checkout") {
            steps {
                git url: 'https://github.com/abhijeetatmindstix/android-demo-app-2.git'
            }
        }
        stage("Build debug APK") {
            steps {
                sh "./gradlew assembleDebug"
            }
        }
        stage("Build release APK") {
            steps {
                sh "./gradlew bundleRelease"
    
            }
        } 
         stage('Compile') {
            archiveArtifacts artifacts: '**/*.apk', fingerprint: true, onlyIfSuccessful: true            
        }
    }
}






pipeline {
   agent any
   
   
 
 stages {
        stage('Build') {
            steps {
                sh './gradlew assembleDebug'
            }
        }
        
        
        stage('Test') {
            steps {
                sh './gradlew test'
            }
        }
        

              
    }
    post {
        success {
            archiveArtifacts artifacts: 'app/build/outputs/apk/debug/*.apk', fingerprint: true
        }
    }
}


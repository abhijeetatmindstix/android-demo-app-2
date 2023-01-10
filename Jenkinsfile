
                    
//     }
//     post {
//         success {
//             archiveArtifacts artifacts: 'app/build/outputs/apk/debug/*.apk', fingerprint: true
//         }
//     }
// }

// }







pipeline {
    agent any
    }
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
       
       stage('build'){
          steps{
            sh './gradlew assembleDebug'
          } 
       
       }   
              
    }
}

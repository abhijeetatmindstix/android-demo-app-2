pipeline {
    agent any

    options {
        disableConcurrentBuilds(abortPrevious: true)
    }

    stages {
        
        stage('Preparation') {
            options {timestamps () }
            steps {
                echo "Starting Gradle daemon"
                sh './gradlew --no-daemon --version'
            }
        }
        
        stage('Check ADB and Gradle') {
            options {timestamps () }
            steps {
                sh 'ps aux | grep adb'
                sh 'ps aux | grep gradlew'
            }
        }
        stage('Start Emulator') {
            options {timestamps () }
            steps {
                sh '/opt/homebrew/bin/adb start-server'
            }
        }
  
        stage('Checkout') {
            options {timestamps () }
            steps {
                echo "Starting Checkout stage"
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], 
                userRemoteConfigs: [[url: 'https://github.com/abhijeetatmindstix/android-demo-app-2.git']]])
            }
        }
        
        
        // stage('Bundle') {
        //     options {timestamps () }
        //     steps {
               
        //         sh './gradlew bundleRelease'
        //     }
        // }
        // stage('Test') {
        //     options {timestamps () }
        //     steps {
        //         // sh './gradlew test'
        //         sh './gradlew --no-daemon --offline test'
        //     }
        // }
        // stage('Code Analysis') {
        //     options {timestamps () }
        //     steps {
        //         // sh './gradlew check'
        //         sh './gradlew --no-daemon --offline check'
        //     }
        // }        
        // stage('Build') {
        //     options {timestamps () }
        //     steps {
        //         sh './gradlew assembleDebug'
        //     }
        // }
        stage('Parallel Build') {
            options {timestamps () }
            parallel {
                stage('Bundle') {
                    steps {
//                         sh './gradlew --no-daemon --offline bundleRelease'
                        sh './gradlew bundleRelease'
                    }
                }
                stage('Test') {
                    steps {
                        sh './gradlew test'
                    }
                }
                stage('Code Analysis') {
                    steps {
                        sh './gradlew check'
                    }
                }
                stage('Build') {
                    steps {
//                         sh './gradlew --no-daemon --offline assembleDebug'
                        sh './gradlew assembleDebug'
                    }
                }
            }
        }


        stage('Check ADB') {
            options {timestamps () }
            steps {
                sh '/opt/homebrew/bin/adb devices'
            }
        }
        

        stage('Run Device Tests') {
            options {timestamps () }
            steps {
                sh './gradlew connectedDebugAndroidTest'
//                 sh './gradlew --no-daemon --offline connectedDebugAndroidTest'
            }
        }        
       
        stage('Archive') {
            options {timestamps () }
            steps {
                archiveArtifacts artifacts: '**/*.apk' // command to archive the artifacts
            }
        }        

    }
    
    post {
        always {
            milestone(1)
        }
        failure {
            input 'The previous build failed. Do you want to proceed with this build?'
        }
    }   

}



pipeline {
    agent any
    options {
        disableConcurrentBuilds(abortPrevious: true)
    }

    stages {
        stage('Bundle') {
            steps {
                sh "echo 'Current time: \$(date +%Y-%m-%d_%H-%M-%S)'"
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
                sh './gradlew assembleDebug'
            }
        }
        stage('Check ADB') {
            steps {
                sh '/opt/homebrew/bin/adb devices'
            }
        }
        
        stage('Start Emulator') {
            steps {
                sh '/opt/homebrew/bin/adb start-server'
            }
        }
        stage('Run Device Tests') {
            steps {
                sh './gradlew connectedDebugAndroidTest'
            }
        }        
       
        stage('Archive') {
            steps {
                archiveArtifacts artifacts: '**/*.apk' // command to archive the artifacts
            }
        }        

    }
}


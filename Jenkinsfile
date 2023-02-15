pipeline {
    agent any

    options {
        disableConcurrentBuilds(abortPrevious: true)
    }

    stages {
         stage('Git Checkout') {
            steps {
                script {
                    gitClone repository: "${git_repository}", branch: "${params.gitBranch}"
                }
            }
        }
        
        stage('Bundle') {
            options {timestamps () }
            steps {
               
                sh './gradlew bundleRelease'
            }
        }
        stage('Test') {
            options {timestamps () }
            steps {
                sh './gradlew test'
            }
        }
        stage('Code Analysis') {
            options {timestamps () }
            steps {
                sh './gradlew check'
            }
        }        
        stage('Build') {
            options {timestamps () }
            steps {
                sh './gradlew assembleDebug'
            }
        }
        stage('Check ADB') {
            options {timestamps () }
            steps {
                sh '/opt/homebrew/bin/adb devices'
            }
        }
        
        stage('Start Emulator') {
            options {timestamps () }
            steps {
                sh '/opt/homebrew/bin/adb start-server'
            }
        }
        stage('Run Device Tests') {
            options {timestamps () }
            steps {
                sh './gradlew connectedDebugAndroidTest'
            }
        }        
       
        stage('Archive') {
            options {timestamps () }
            steps {
                archiveArtifacts artifacts: '**/*.apk' // command to archive the artifacts
            }
        }        

    }
}


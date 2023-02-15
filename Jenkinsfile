pipeline {
    agent any
    // parameters {
    // string(name: 'repository', defaultValue: 'https://github.com/abhijeetatmindstix/android-demo-app-2.git', description: 'GitHub repository URL')
    // string(name: 'branch', defaultValue: 'main', description: 'Git branch to build')
    // }

    options {
        disableConcurrentBuilds(abortPrevious: true)
    }

    stages {
        //  stage('Git Checkout') {
        //     steps {
        //         script {
        //             gitClone repository: "${repository}", branch: "${params.Branch}"
        //         }
        //     }
        // }
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
                options {timestamps () }
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
        
        // stage('Start Emulator') {
        //     options {timestamps () }
        //     steps {
        //         sh '/opt/homebrew/bin/adb start-server'
        //     }
        // }
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
        // stage('Cache') {
        //     steps {
        //         echo "Caching dependencies at `date`"
        //         cache(name: 'dependencies-cache', paths: '**/build/dependencies') {
        //             sh './gradlew assembleDebug'
        //         }
        //     }
        // }        

    }
}

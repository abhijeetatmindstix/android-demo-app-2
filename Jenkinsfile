        stage('Cache') {
            options {timestamps () }
            steps {
                cache (name: 'gradle-cache', paths: '.gradle') {
                    echo "Cache miss - Running clean build"
                    sh './gradlew --no-daemon clean build'
                }
                cache (name: 'dependencies-cache', paths: 'build/dependencies') {
                    echo "Cache miss - Downloading dependencies"
                    sh './gradlew --no-daemon dependencies'
                }
            }
        }


// In the above code, we have defined a 'Cache' stage in which we have specified-
// two caches - 'gradle-cache' and 'dependencies-cache'. 
// These caches will store the Gradle and dependency files respectively, 
// so that subsequent builds do not have to download them again, thereby reducing build time.


// in each cache block, we have specified a name for the cache and the paths that should be cached. 
// We have then defined the steps that should be executed if the cache is not found (cache miss). 
// If the cache is found, the steps inside the cache block will not be executed, and the cached files will be used instead.

// Note that the Job Cacher plugin needs to be installed on the Jenkins instance for this to work.

// There are several ways to reduce build time in a Jenkins pipeline. Here are a few suggestions:

// Use caching: By caching the dependencies or the results of previous builds, 
// you can reduce the time it takes to build your project. You can use the Jenkins built-in caching feature, 
// or you can use third-party caching plugins such as the Job Cacher Plugin or Result Cache Plugin.

// Parallelize builds: If your build has multiple tasks that can be run in parallel, 
// you can split them into separate stages and run them simultaneously. Jenkins provides the Parallel Stages plugin to achieve this.

// Use a faster machine: If your Jenkins server is running on a slow machine, 
// you can upgrade to a faster machine or increase the CPU and memory resources allocated to the Jenkins server.

// Optimize build scripts: Optimize your build scripts to run only the necessary tasks and avoid unnecessary ones. 
// Use the --no-daemon flag to start Gradle faster, and use --offline to avoid downloading dependencies that are already in the local cache.

// Remove unnecessary build steps: Remove any unnecessary build steps or tests that are not needed. 
// For example, you can skip UI tests during the build and run them only during manual testing.

// Use a cloud-based build service: Consider using a cloud-based build service such as 
// Jenkins X or CircleCI to offload the build process from your local Jenkins server and improve build times.

// Implementing these suggestions should help you reduce the build time of your Jenkins pipeline.



To implement parallel builds in Jenkins, you can use the parallel step within a stage. Here is an example Jenkinsfile:

pipeline {
  agent any
  stages {
    stage('Build') {
      parallel {
        stage('Build A') {
          steps {
            sh './gradlew clean build'
          }
        }
        stage('Build B') {
          steps {
            sh './gradlew clean build'
          }
        }
        stage('Build C') {
          steps {
            sh './gradlew clean build'
          }
        }
      }
    }
  }
}
In this example, the Build stage has three parallel sub-stages, each building the same project with different tasks. Jenkins will run all three sub-stages simultaneously, which can help reduce build times.

You can adjust the number of parallel sub-stages to suit your build needs. It's recommended to use the number of available cores or nodes to achieve the best performance.



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
        stage('Cache') {
            options {timestamps () }
            steps {
                caches (name: 'gradle-cache', paths: '.gradle') {
                    echo "Cache miss - Running clean build"
                    sh './gradlew --no-daemon --offline clean build'
                }
                caches (name: 'dependencies-cache', paths: 'build/dependencies') {
                    echo "Cache miss - Downloading dependencies"
                    sh './gradlew --no-daemon --offline dependencies'
                }
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
        stage('Parallel Build') {
            options {timestamps () }
            parallel {
                stage('Bundle') {
                    steps {
                        sh './gradlew --no-daemon --offline bundleRelease'
                    }
                }
                stage('Test') {
                    steps {
                        sh './gradlew --no-daemon --offline test'
                    }
                }
                stage('Code Analysis') {
                    steps {
                        sh './gradlew --no-daemon --offline check'
                    }
                }
                stage('Build') {
                    steps {
                        sh './gradlew --no-daemon --offline assembleDebug'
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
                sh './gradlew --no-daemon --offline connectedDebugAndroidTest'
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
        failure {
            script {
                currentBuild.result = 'FAILURE'
                error 'Previous build failed'
            }
        }
    }     

}




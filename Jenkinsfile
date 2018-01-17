#!/usr/bin/env groovy

pipeline {

    agent {
        docker {
            image 'golang:1.8.0-alpine'
            args '-v ${pwd()}:/go/src/github.com/alstard/GoJenkins'
        }
    }

    stages {
        stage('Checkout from GitHub') {
            steps {
                checkout scm
            }
            
        }
        stage("Create binaries") {
            steps {
                docker.image("golang:1.8.0-alpine").inside("-v ${pwd()}:${goPath}") {
        
                    for (command in binaryBuildCommands) {
                        // build the Mac x64 binary
                        sh "cd /go/src/github.com/alstard/GoJenkins && GOOS=darwin GOARCH=amd64 go build -o binaries/amd64/0.1.0/darwin/${applicationName}-${buildNumber}.darwin.amd64"

                        // build the Windows x64 binary
                        sh "cd /go/src/github.com/alstard/GoJenkins && GOOS=windows GOARCH=amd64 go build -o binaries/amd64/0.1.0/windows/${applicationName}-${buildNumber}.windows.amd64.exe"

                        // build the Linux x64 binary
                        sh "cd /go/src/github.com/alstard/GoJenkins && GOOS=linux GOARCH=amd64 go build -o binaries/amd64/0.1.0/linux/${applicationName}-${buildNumber}.linux.amd64"
                    }
                }
            }
        }
    }
}
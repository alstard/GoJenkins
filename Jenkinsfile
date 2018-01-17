#!/usr/bin/env groovy

pipeline {

    agent {
        docker {
            image 'golang:1.8.0-alpine'
            args '-v /go/src/github.com/alstard/GoJenkins:/go/src/github.com/alstard/GoJenkins'
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
                        sh "cd /go/src/github.com/alstard/GoJenkins && GOOS=darwin GOARCH=amd64 go build -o binaries/amd64/0.1.0/darwin/GoJenkins-v.darwin.amd64"
                        sh "cd /go/src/github.com/alstard/GoJenkins && GOOS=windows GOARCH=amd64 go build -o binaries/amd64/0.1.0/windows/GoJenkins-0.1.0.windows.amd64.exe"
                        sh "cd /go/src/github.com/alstard/GoJenkins && GOOS=linux GOARCH=amd64 go build -o binaries/amd64/0.1.0/linux/GoJenkins-0.1.0.linux.amd64"
                    }
                }
    }
}

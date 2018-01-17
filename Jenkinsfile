#!/usr/bin/env groovy

// Setup variables
// application name will be used in a few places so create a variable and use string interpolation to use it where needed
String applicationName = "GoJenkins"
// a basic build number so that when we build and push to Artifactory we will not overwrite our previous builds
String buildNumber = "0.1.${env.BUILD_NUMBER}"
// Path we will mount the project to for the Docker container
String goPath = "/go/src/github.com/alstard/${applicationName}"

pipeline {

    agent {
        docker {
            image 'golang:1.8.0-alpine'
            args '-v ${pwd()}:${goPath}'
        }
    }

    stages {
        stage('Checkout from GitHub') {
            checkout scm
        }
        stage("Create binaries") {
            docker.image("golang:1.8.0-alpine").inside("-v ${pwd()}:${goPath}") {
      
                for (command in binaryBuildCommands) {
                    // build the Mac x64 binary
                    sh "cd ${goPath} && GOOS=darwin GOARCH=amd64 go build -o binaries/amd64/${buildNumber}/darwin/${applicationName}-${buildNumber}.darwin.amd64"

                    // build the Windows x64 binary
                    sh "cd ${goPath} && GOOS=windows GOARCH=amd64 go build -o binaries/amd64/${buildNumber}/windows/${applicationName}-${buildNumber}.windows.amd64.exe"

                    // build the Linux x64 binary
                    sh "cd ${goPath} && GOOS=linux GOARCH=amd64 go build -o binaries/amd64/${buildNumber}/linux/${applicationName}-${buildNumber}.linux.amd64"
                }
            }
        }
    }
}
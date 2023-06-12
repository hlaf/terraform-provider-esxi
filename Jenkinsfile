#!groovy

@Library('emt-pipeline-lib@latest') _

repo_creds = 'emt-jenkins-github-ssh'
repo_url = 'git@github.com:hlaf/terraform-provider-esxi'
branch = 'v1.8.2-bugfixes'

node('docker-slave') {

    stage('Checkout') {
        checkoutFromGit(repo_creds, repo_url, branch)
    }

    stage('Build') {
        def build_tools_image = 'golang:1.20.5-alpine3.18'
        sh """docker run -t --volumes-from $DOCKER_CONTAINER_ID $build_tools_image \
            /bin/sh -c 'cd ${env.WORKSPACE};\
            mkdir build;\
            go env;\
            CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -a -ldflags "-w -extldflags \"-static\"" -o build/terraform-provider-esxi'
        """
        sh 'ls -al build/'
        sh 'git status'
    }

    stage('Release') {}
}

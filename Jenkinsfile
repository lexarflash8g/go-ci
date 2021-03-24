pipeline {
    agent any
    tools {
        go 'default'
    }
    environment {
        GO111MODULE = 'on'
    }
    stages {
        stage('Build') {
            steps {
                sh 'go build'
            }
        }
    

stage('Test') {
    environment {
        CODECOV_TOKEN = credentials('CODECOV_TOKEN')
    }
    steps {
        sh 'go test ./... -coverprofile=coverage.txt'
        sh "curl -s https://codecov.io/bash | bash -s -"
    }
}

stage('Code Analysis') {
    steps {
        sh 'curl -sfL https://install.goreleaser.com/github.com/golangci/golangci-lint.sh | sudo bash -s -- -b $GOPATH/bin v1.17.1'
        sh 'sudo golangci-lint run'
    }
}


stage('Release') {
    when {
        buildingTag()
    }
    environment {
        GITHUB_TOKEN = credentials('GITHUB_TOKEN')
    }
    steps {
        sh 'curl -sL https://git.io/goreleaser | bash'
    }
}


}
}

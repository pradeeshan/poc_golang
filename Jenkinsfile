pipeline {
    agent { label 'mac' }

    tools {
        go 'Go-1.22'
    }

    environment {
        APP_NAME = "go-pipeline-poc"
        BUILD_DIR = "build"
    }

    stages {
        stage('Build') {
            steps {
                sh '''
                    go version
                    mkdir -p ${BUILD_DIR}
                    go mod tidy
                    go build -o ${BUILD_DIR}/${APP_NAME}
                '''
            }
        }

        stage('Test') {
            steps {
                sh 'go test ./... -v'
            }
        }

        stage('Deploy (Local)') {
            steps {
                sh '''
                    chmod +x ${BUILD_DIR}/${APP_NAME}
                    nohup ./${BUILD_DIR}/${APP_NAME} > app.log 2>&1 &
                '''
            }
        }
    }
}


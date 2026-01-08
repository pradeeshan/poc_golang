pipeline {
    agent any

    environment {
        GO_ENV = "local"
        APP_NAME = "go-pipeline-poc"
        BUILD_DIR = "build"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Checking out source code"
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Building Go application"
                sh '''
                    mkdir -p ${BUILD_DIR}
                    go mod tidy
                    GOOS=darwin GOARCH=amd64 go build -o ${BUILD_DIR}/${APP_NAME}
                '''
            }
        }

        stage('Test') {
            steps {
                echo "Running Go tests"
                sh '''
                    go test ./... -v
                '''
            }
        }

        stage('Deploy (Local)') {
            steps {
                echo "Deploying locally on Mac"
                sh '''
                    chmod +x ${BUILD_DIR}/${APP_NAME}
                    nohup ./${BUILD_DIR}/${APP_NAME} > app.log 2>&1 &
                    echo "App started locally"
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully"
        }
        failure {
            echo "Pipeline failed"
        }
    }
}

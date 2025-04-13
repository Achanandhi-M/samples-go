pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'chore/include-jenkins-pipeline-for-go-app', url: 'https://github.com/Achanandhi-M/samples-go'
            }
        }

        stage('Build') {
            steps {
                dir('gin-mongo') {
                    sh 'go mod tidy'
                    sh 'go build -v ./...'
                }
            }
        }

        stage('Keploy Test') {
            steps {
                sh "curl --silent --location 'https://github.com/keploy/keploy/releases/latest/download/keploy_linux_arm64.tar.gz' | tar xz --overwrite -C /tmp"
                sh "mkdir -p /usr/local/bin && sudo mv /tmp/keploy /usr/local/bin/keploy"

                dir('gin-mongo') {
                    sh '''
                        keploy test -c "docker compose up" --container-name "ginMongoApp" --delay 10
                    '''
                }
            }
        }
    }
}

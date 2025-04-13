pipeline {
    agent any
    stages {
        stage('Keploy Tests') {
            steps {

                // Clone the repository
                git branch: 'main', url: 'https://github.com/Achanandhi-M/samples-go'

                // Download and prepare Keploy binary
                sh "curl --silent --location 'https://github.com/keploy/keploy/releases/latest/download/keploy_linux_arm64.tar.gz' | tar xz --overwrite -C /tmp"
                sh "mkdir -p /usr/local/bin && sudo mv /tmp/keploy /usr/local/bin/keploy"

                // switch to the directory where keploy folder is present
                dir('gin-mongo'){

                // Make sure you have NPM in host machine and Install application dependencies.

                sh"""
                keploy test -c "docker compose up" --container-name "ginMongoApp" --delay 10
                """
              }
            }
        }
    }
}
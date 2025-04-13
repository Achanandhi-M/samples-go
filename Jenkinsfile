pipeline {
    agent any
    stages {
        stage('Keploy Tests') {
            steps {

                // Clone the git repository
                git branch: 'chore/include-jenkins-pipeline-for-go-app', url: 'https://github.com/Achanandhi-M/samples-go'

                // Download and prepare Keploy binary
                sh "curl --silent --location 'https://github.com/keploy/keploy/releases/latest/download/keploy_linux_arm64.tar.gz' | tar xz  -C /tmp"
                sh 'sudo mkdir -p /usr/local/bin && sudo mv /tmp/keploy /usr/local/bin/keploy'

                // switch to the directory where keploy folders is present and run the test
                dir('gin-mongo'){

                sh"""
                sudo -E keploy test -c "docker compose up" --container-name "ginMongoApp" --delay 15
                """
              }
            }
        }
    }
}
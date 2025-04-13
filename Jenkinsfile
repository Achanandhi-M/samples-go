pipeline {
    agent any
    stages {
        stage('Keploy Tests') {
            steps {
                // Clone the git repository
                git branch: 'chore/include-jenkins-pipeline-for-go-app', url: 'https://github.com/Achanandhi-M/samples-go'

                // Download and install Keploy binary
                sh """
                curl --silent -O -L https://keploy.io/install.sh
                chmod +x install.sh
                ./install.sh
                export PATH="$HOME/.keploy/bin:$PATH"
                """

                // switch to the directory and run tests
                dir('gin-mongo') {
                    sh """
                    export PATH="$HOME/.keploy/bin:$PATH"
                    keploy test -c "docker compose up" --container-name "ginMongoApp" --delay 15
                    """
                }
            }
        }
    }
}
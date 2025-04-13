pipeline {
    agent any
    
    environment {
        // Set PATH globally for all steps
        PATH = "/usr/local/bin:${HOME}/.keploy/bin:${PATH}"
    }
    
    stages {
        stage('Keploy Tests') {
            steps {
                git branch: 'chore/include-jenkins-pipeline-for-go-app', 
                url: 'https://github.com/Achanandhi-M/samples-go'

                sh '''
                    echo "### Verifying Environment ###"
                    echo "PATH: $PATH"
                    which docker
                    which keploy
                    docker --version
                '''

                // Install Keploy if not already installed
                sh '''
                    if ! command -v keploy >/dev/null; then
                        echo "Installing Keploy..."
                        curl --silent -O -L https://keploy.io/install.sh
                        chmod +x install.sh
                        ./install.sh
                    fi
                '''

                dir('gin-mongo') {
                    sh '''
                        echo "### Current PATH inside gin-mongo ###"
                        echo "$PATH"
                        
                        echo "### Running Keploy Test ###"
                        keploy test -c "docker compose up" --container-name "ginMongoApp" --delay 15
                    '''
                }
            }
        }
    }
}
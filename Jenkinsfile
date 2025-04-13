pipeline {
    agent any
    stages {
        stage('Keploy Tests') {
            steps {
                git branch: 'chore/include-jenkins-pipeline-for-go-app', 
                url: 'https://github.com/Achanandhi-M/samples-go'
                sh '''
                    export PATH="/usr/local/bin:$PATH"  # Homebrew install path
                    docker --version  # Verify Docker
                    
                    # Install Keploy if not present
                    if ! command -v keploy &> /dev/null; then
                        curl --silent -O -L https://keploy.io/install.sh
                        chmod +x install.sh
                        ./install.sh
                        export PATH="$HOME/.keploy/bin:$PATH"
                    fi
                '''

                dir('gin-mongo') {
                    sh '''
                        export PATH="/usr/local/bin:$HOME/.keploy/bin:$PATH"
                        keploy test -c "docker compose up" --container-name "ginMongoApp" --delay 15
                    '''
                }
            }
        }
    }
}
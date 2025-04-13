pipeline {
    agent any
    stages {
        stage('Setup Environment') {
            steps {
                // Install Docker if not present (for Debian/Ubuntu agents)
                sh """
                if ! command -v docker &> /dev/null; then
                    echo "Installing Docker..."
                    sudo apt-get update
                    sudo apt-get install -y docker.io
                    sudo usermod -aG docker \$USER
                    newgrp docker
                fi
                """
            }
        }
        
        stage('Keploy Tests') {
            steps {
                // Clone the git repository
                git branch: 'chore/include-jenkins-pipeline-for-go-app', url: 'https://github.com/Achanandhi-M/samples-go'

                // Download and install Keploy binary
                sh """
                curl --silent -O -L https://keploy.io/install.sh
                chmod +x install.sh
                ./install.sh
                export PATH="\$HOME/.keploy/bin:\$PATH"
                """

                // Verify installations
                sh """
                echo "Docker version: \$(docker --version)"
                echo "Keploy version: \$(keploy version)"
                """

                // switch to the directory and run tests
                dir('gin-mongo') {
                    sh """
                    export PATH="\$HOME/.keploy/bin:\$PATH"
                    keploy test -c "docker compose up" --container-name "ginMongoApp" --delay 15
                    """
                }
            }
        }
    }
}
pipeline {
    agent any
    stages {
        stage('Setup Environment') {
            steps {
                // Install Docker if not present (for different OS environments)
                sh '''
                if ! command -v docker &> /dev/null; then
                    echo "Installing Docker..."
                    # Check the OS and install Docker accordingly
                    if [[ "$(uname)" == "Darwin" ]]; then
                        echo "Detected macOS, installing Docker for macOS"
                        brew install --cask docker
                    elif [[ -f /etc/debian_version ]]; then
                        echo "Detected Debian/Ubuntu, installing Docker"
                        sudo apt-get update
                        sudo apt-get install -y docker.io
                    else
                        echo "Unsupported OS for automatic Docker installation"
                        exit 1
                    fi
                    # Add user to the Docker group if applicable
                    sudo usermod -aG docker $USER
                    newgrp docker
                fi
                '''
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

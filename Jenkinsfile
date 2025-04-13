pipeline {
    agent {
        docker {
            image 'jenkins/jenkins:lts'
            args '-u root -v /var/run/docker.sock:/var/run/docker.sock -v $(which docker):/usr/bin/docker'
        }
    }
    
    environment {
        PATH = "/usr/local/bin:/usr/bin:${env.HOME}/.keploy/bin:${env.PATH}"
        DOCKER_HOST = "unix:///var/run/docker.sock"
    }
    
    stages {
        stage('Verify Docker') {
            steps {
                sh '''
                    echo "Checking Docker access..."
                    ls -l /var/run/docker.sock
                    docker --version
                '''
            }
        }
        
        // Rest of your stages remain the same
        stage('Checkout Code') {
            steps {
                git branch: 'chore/include-jenkins-pipeline-for-go-app', 
                url: 'https://github.com/Achanandhi-M/samples-go'
            }
        }
        
        stage('Install Keploy') {
            steps {
                sh '''
                    echo "### Verifying Environment ###"
                    echo "PATH: $PATH"
                    which docker
                    docker --version
                    
                    if ! command -v keploy >/dev/null; then
                        echo "Installing Keploy..."
                        curl --silent -O -L https://keploy.io/install.sh
                        chmod +x install.sh
                        ./install.sh
                    else
                        echo "Keploy is already installed"
                    fi
                    
                    echo "Keploy version:"
                    keploy --version
                '''
            }
        }
        
        stage('Run Keploy Tests') {
            steps {
                dir('gin-mongo') {
                    sh '''
                        echo "### Current PATH inside gin-mongo ###"
                        echo "$PATH"
                        
                        echo "### Running Keploy Test ###"
                        keploy test -c "docker-compose up" --container-name "ginMongoApp" --delay 15
                    '''
                }
            }
        }
    }
}
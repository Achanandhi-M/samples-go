pipeline {
    agent any
    stages {
        stage('Keploy Tests') {
            steps {
                sh '''
                sudo apt-get update && sudo apt-get install -y curl kmod linux-headers-generic bpfcc-tools git go
                # Clone the git repository
                git clone 'https://github.com/Achanandhi-M/samples-go.git'
                cd gin-mongo

                # Create directories required by eBPF

                sudo mkdir -p /sys/kernel/debug
                sudo mkdir -p /sys/kernel/tracing

                # Download and install Keploy binary
                curl --silent -O -L https://keploy.io/install.sh && bash install.sh

                # Mount debugfs and tracefs if not already mounted
                sudo mount -t debugfs nodev /sys/kernel/debug || true
                sudo mount -t tracefs nodev /sys/kernel/tracing || true

                go mod tidy
                keploy test -c " "go run main.go handler.go" --delay 40
                '''
                }
            }
        }
    }
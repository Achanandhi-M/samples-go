pipeline {
    agent any
    options {
        ansiColor('xterm')
    }
    stages {
        stage('Keploy Tests') {
            steps {
                sh '''
                sudo apt-get update && sudo apt-get install -y curl kmod linux-headers-generic bpfcc-tools git golang-go
                # Remove existing repo directory if it exists
                rm -rf samples-go
                git clone 'https://github.com/Achanandhi-M/samples-go.git'
                cd gin-mongo

                sudo mkdir -p /sys/kernel/debug
                sudo mkdir -p /sys/kernel/tracing

                curl --silent -O -L https://keploy.io/install.sh && bash install.sh

                sudo mount -t debugfs nodev /sys/kernel/debug || true
                sudo mount -t tracefs nodev /sys/kernel/tracing || true

                go mod tidy
                keploy test -c "go run main.go handler.go" --delay 30  --disableANSI
                '''
            }
        }
    }
}

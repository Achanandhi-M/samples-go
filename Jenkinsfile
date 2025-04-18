pipeline {
    agent any
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'sudo apt-get update && sudo apt-get install -y curl kmod linux-headers-generic bpfcc-tools git golang-go'
            }
        }
        stage('Clone and Setup App') {
            steps {
                sh '''
                rm -rf samples-go
                git clone 'https://github.com/Achanandhi-M/samples-go'
                cd gin-mongo
                go mod tidy
                '''
            }
        }
        stage('Install Keploy') {
            steps {
                sh '''
                curl --silent -O -L https://keploy.io/install.sh && bash install.sh
                '''
            }
        }
        stage('Prepare eBPF Hooks') {
            steps {
                sh '''
                sudo mkdir -p /sys/kernel/debug
                sudo mkdir -p /sys/kernel/tracing
                sudo mount -t debugfs nodev /sys/kernel/debug || true
                sudo mount -t tracefs nodev /sys/kernel/tracing || true
                '''
            }
        }
        stage('Run Keploy Tests') {
            steps {
                sh '''
                cd gin-mongo
                sudo -E keploy test -c "go run main.go handler.go" --disableANSI
                '''
            }
        }
    }
}

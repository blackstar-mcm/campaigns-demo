pipeline {
    agent {
        node {
            label 'dockerhost-build-server'
        }
    }
    tools {
        maven 'maven-3.9.6'
    }
    stages {
        stage('Packaging') {
            steps {
                echo 'Packaging...'
                sh 'mvn clean package'
            }
        }
        stage('Copying jar file') {
            steps {
                echo 'Copying jar file...'
                sh 'mv target/*.jar .'
            }
        }
        stage('Cleanup Docker images and volumes') {
            steps {
                echo 'Cleaning up unused Docker images and volumes...'
                sh 'docker system prune -a --volumes --force --filter "label=campaign-demo-server"'
            }
        }
        stage('Build Docker image') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t miichellecame/campaign-demo:v1 --label campaign-demo-server .'
            }
        }
        stage('Run Docker container') {
            steps {
                echo 'Running Docker container...'
                sh '''
                    if [ "$(docker ps -aq -f name=campaign-demo-server)" ]; then
                        echo "Stopping and removing existing container..."
                        docker rm -f campaign-demo-server
                    fi

                    docker run -d --name campaign-demo-server --label campaign-demo-server -p 8081:8081 miichellecame/campaign-demo:v1
                '''
            }
        }
    }
}

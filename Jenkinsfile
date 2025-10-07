pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                echo "📥 Pulling latest code from GitHub..."
                git branch: 'master', url: 'https://github.com/rakshanda/profile_app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "🐳 Building Docker image..."
                sh 'docker build -t profile-app:latest .'
            }
        }

        stage('Run Docker Container') {
            steps {
                echo "🚀 Running Docker container..."
                script {
                    // Clean up existing container if it exists
                    sh '''
                        if [ "$(docker ps -aq -f name=profile-app-container)" ]; then
                            echo "🧹 Removing existing container..."
                            docker rm -f profile-app-container
                        fi
                    '''

                    // Start new container with proper port mapping
                    sh '''
                        echo "🟢 Starting new container on port 5000..."
                        docker run -d --name profile-app-container -p 5000:5000 profile-app:latest
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment completed successfully!"
            sh 'docker images'
            sh 'docker ps -a'
        }
        failure {
            echo "❌ Deployment failed. Please check logs."
            sh 'docker ps -a'
        }
    }
}

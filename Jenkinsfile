pipeline {
    agent any
    stages {
        stage('Cloning Git Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/ericknyatenya/jk-public-gh.git'
            }
        }
        
        stage('Building Image') {
            steps {
                sh 'docker build -t webapp:${BUILD_NUMBER} .'
            }
        }
        
        stage('Deploying Application') {
            steps {
                sh '''
                    # Stop and remove existing container if it exists
                    if [ $(docker ps -q -f name=webapp_ctr) ]; then
                        echo "Stopping existing container..."
                        docker stop webapp_ctr
                    fi
                    
                    if [ $(docker ps -aq -f name=webapp_ctr) ]; then
                        echo "Removing existing container..."
                        docker rm webapp_ctr
                    fi
                    
                    # Run new container
                    echo "Starting new container with image webapp:${BUILD_NUMBER}"
                    docker run --rm -d -p 3000:3000 --name webapp_ctr webapp:${BUILD_NUMBER}
                    
                    # Verify container is running
                    docker ps | grep webapp_ctr
                '''    
            }
        }
    }
    
    post {
        always {
            // Clean up old images to save space (optional)
            sh 'docker image prune -f'
        }
    }
}

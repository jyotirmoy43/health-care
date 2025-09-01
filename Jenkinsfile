pipeline {
    agent any

    tools {
        // Make sure this matches the Maven installation name in Jenkins (Global Tool Configuration)
        maven '3.6.3'  // <-- CHANGE THIS if your tool is named differently (e.g., 'Maven_3.6.3')
    }

    environment {
        MAVEN_OPTS = "-Dmaven.repo.local=.m2/repository"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/jyotirmoy43/health-care.git'
                echo '✅ Checked out code from GitHub'
            }
        }

        stage('Verify Maven') {
            steps {
                echo '🔍 Verifying Maven installation...'
                sh 'mvn -v'
            }
        }

        stage('Compile Code') {
            steps {
                echo '🔨 Compiling source code...'
                sh 'mvn compile'
            }
        }

        stage('Run Tests') {
            steps {
                echo '🧪 Running unit tests...'
                sh 'mvn test'
            }
        }

        stage('QA - Code Quality') {
            steps {
                echo '📊 Running Checkstyle...'
                sh 'mvn checkstyle:checkstyle'
            }
        }

        stage('Package Application') {
            steps {
                echo '📦 Packaging application...'
                sh 'mvn package'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🐳 Building Docker image...'
                sh 'docker build -t jyotirmoy43/myimg1 .'
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                echo '📤 Pushing Docker image to Docker Hub...'
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push jyotirmoy43/myimg1
                    '''
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                echo '🚀 Running Docker container...'
                sh '''
                    docker rm -f c21 || true
                    docker run -dt -p 8092:8092 --name c21 jyotirmoy43/myimg1
                '''
            }
        }
    }
}

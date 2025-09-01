pipeline {
    agent any

    tools {
        maven 'Maven_3.6.3'
    }

    environment {
        MAVEN_OPTS = "-Dmaven.repo.local=.m2/repository"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/jyotirmoy43/health-care.git'
                echo 'Checked out code from GitHub'
            }
        }

        stage('Compile Code') {
            steps {
                echo 'Starting compilation...'
                sh 'mvn compile'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('QA - Code Quality') {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }

        stage('Package Application') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t jyotirmoy43/myimg1 .'
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
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
                sh '''
                    docker rm -f c000 || true
                    docker run -dt -p 8091:8091 --name c000 jyotirmoy43/myimg1
                '''
            }
        }
    }
}

pipeline {
    agent any

    environment {
        NODE_ENV = 'production'
        IMAGE_NAME = 'your-dockerhub-username/node-app'
    }

    options {
        timestamps()
        ansiColor('xterm')
    }

    stages {
        stage('Pull Coded') {
            steps {
                git branch: 'main', url: 'https://github.com/abdelrahmanonline4/sourcecode.git'
            }
        }

        stage('install dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('build code') {
            steps {
                sh 'npm run build'
            }
        }

        stage('build, tag and push the image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                docker build -t $DOCKER_USER/senior-node-image:latest .
                docker push $DOCKER_USER/senior-node-image:latest
            '''
                }
            }
        }
    }

}

pipeline {
    agent any

    tools {
        nodejs 'node-js'   // must match the name you set under Manage Jenkins → Tools
    }

    stages {
        stage('Pull Coded') {
            steps {
                git branch: 'main', url: 'https://github.com/mohamedbstar413/senior'
            }
        }

        stage('Debug'){
            steps{
                sh 'ls'
            }
        }

        stage('install dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('build, tag and run the image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                docker build -t $DOCKER_USER/solar-image:latest .
                docker run -d --name senior-jenkins -p 3000:3000 senior-node-image
            '''
                }
            }
        }
    }
}

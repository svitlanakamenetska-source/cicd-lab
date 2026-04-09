pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    stages {

        stage('Set variables') {
            steps {
                script {
                    if (env.BRANCH_NAME == "main") {
                        env.IMAGE = "nodemain:v1.0"
                        env.PORT = "3000"
                    } else {
                        env.IMAGE = "nodedev:v1.0"
                        env.PORT = "3001"
                    }
                }
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE .'
            }
        }

        stage('Deploy') {
            steps {
                sh """
                docker ps -q --filter "publish=$PORT" | xargs -r docker stop
                docker ps -a -q --filter "publish=$PORT" | xargs -r docker rm
                docker run -d -p $PORT:3000 $IMAGE
                """
            }
        }
    }
}

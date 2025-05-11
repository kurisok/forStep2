pipeline {
    agent { label 'jenkins-worker' }

    stages {
        stage('Pull Code') {
            steps {
                git branch: 'main', url: 'https://github.com/kurisok/forStep2.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('my-image')
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    docker.image('my-image').inside {
                        sh 'npm test'
                    }
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    if (currentBuild.result == 'SUCCESS') {
                        docker.withRegistry('https://hub.docker.com', 'docker-credentials-id') {
                            docker.image('my-image').push()
                        }
                    } else {
                        echo 'Tests failed'
                    }
                }
            }
        }
    }
}
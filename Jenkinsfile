pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    // Replace <DOCKER_HUB_USERNAME> with your actual Docker Hub username
                    app = docker.build("jahnavi1999/train-schedule")
                    app.inside {
                        sh 'echo $(curl localhost:8080)'  // Test if the app is running inside the container
                    }
                }
            }
        }
        stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    // Ensure 'docker_hub_login' is the correct credentials ID in Jenkins
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
    }   
}

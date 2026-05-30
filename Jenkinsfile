pipeline {
    agent any

    stages {
        
        stage('Code Cloning') {
            steps {
                git url: "https://github.com/RJadhav2004/two-tier-flask-app-1.git", branch: "main"
            }
        }
        stage('Build Image') {
            steps {
                sh "docker build -t two-tier-flask-app ."
            }
        }
        stage('Test') {
            steps {
                echo 'Testing code'
            }
        }
        stage('Image push to dockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCreds",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                )]){
                
                sh "docker login -u ${env.dockerHubUser} -p ${dockerHubPass}"
                sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                }
            }
        }
        stage('App Deploy') {
            steps {
                sh "docker compose up -d --build flask-app"
            }
        }
        
    }
}

pipeline {
    agent { label "dev" }
    stages {
        stage("Code Clone") {
            steps {
                git url: "https://github.com/YYash-Rathore/two-tier-flask-app.git", branch: "master"
            }
        }
        stage("Build") {
            steps {
                sh "docker build -t two-tier-flask-app ."
            }
        }
        stage("Testing") {
            steps {
                echo "Testing ho gayi"
            }
        }
        stage("Push to Docker Hub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCreds",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                    )]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                    sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"    
                }
            }
        }
        stage("Cleanup") {
            steps {
                sh 'docker rm -f mysql || true'  // Ignore error if container doesn't exist
                sh 'docker rm -f flask-app || true'
            }
        }
        stage("Deploy") {
            steps {
                
                sh "docker compose up -d --build flask-app"
            }
        }
    }
}

pipeline{
    agent any;
    stages{
        stage("code"){
            steps{
                git url: "https://github.com/csrirammganesh/two-tier-flask-app.git", branch:"master"
            }
        }
        stage("build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
        }
        stage("docker push"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"DockerHubCreds",
                    passwordVariable: "dockerhubpass",
                    usernameVariable: "dockerhubuser"
                )])
                {
                sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubpass}"
                sh "docker image tag two-tier-flask-app ${env.dockerhubuser}/two-tier-flask-app:latest"
                sh "docker push ${env.dockerhubuser}/two-tier-flask-app:latest"
            
                }    
            }
        }
        stage("deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }
}

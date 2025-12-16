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
                sh "docker build -t two-tier-sriram-app ."
            }
        }
        stage("docker push"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId: "DockerHubUser",
                    usernameVariable: "dockerhubuser",
                    passwordVariable: "dockerhubpass"
                )]){
                sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                sh "docker image tag two-tier-sriram-app ${env.dockerhubuser}/two-tier-sriram-app"
                sh "docker push ${env.dockerhubuser}/two-tier-sriram-app:latest"
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

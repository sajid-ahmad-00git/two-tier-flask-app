pipeline{
    agent {label "dev"} 
    stages{
        stage("code clone"){
            steps{
                git url: "https://github.com/sajid-ahmad-00git/two-tier-flask-app.git", branch: "master"
            }
        }
        stage("Build"){
            steps{
                sh "docker buil -t two-tier-flask-app  ."
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials([
                    usernamePassword(credentialsId: "dockerCred",
                    passwordVariable: "dockerPass", 
                    usernameVariable: "dockerUser"
                )]){
                sh "docker login -u ${env.dockerUser} -p ${env.dockerPass}"
                sh "docker image tag two-tier-flask-app ${env.dockerUser}/two-tier-flask-app"
                sh "docker push ${env.dockerUser}/two-tier-flask-app:latest"
                }
                
            }
        }
        stage("Test"){
            steps{
                echo "developer will test that works or not"
            }
        }
        stage("deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }
    post{
        success{
            script{
                emailext from: "sajid.ahmad.lrn@gmail.com",
                to: "sajid.ahmad.lrn@gmail.com",
                body: " Build is successful",
                subject: " Build configuration"
            }
        }
        failure{
            script{
                emailext from: "sajid.ahmad.lrn@gmail.com",
                to: "sajid.ahmad.lrn@gmail.com",
                body: " Build Failed miserably",
                subject: " Build configuration"
            }
        }
        
    }
}

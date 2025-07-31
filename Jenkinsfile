pipeline{
    agent { label "dev";
    
    stages{
        stage("Code checkout"){
            steps{
                git url: "https://github.com/shreeaws/two-tier-flask-app.git", branch: "master"
            }
        }
        stage("Building the docker image"){
            steps{
                sh "docker build -t flask-app ."
            }
        }
        stage("Unit testing"){
            steps{
                echo "Developer / Tester testing script dega"
            }
        }
        stage("Push to Docker Hub"){
            steps{
                withCredentials([usernamePassword(
                        credentialsId:"dockerHubCreds",
                        passwordVariable:"dockerHubPass",
                        usernameVariable:"dockerHubUser"
                        )])
                        {
                    
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag flask-app ${env.dockerHubUser}/flask-app"
                sh "docker push ${env.dockerHubUser}/flask-app:latest"
                        }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d"
            }
        }
    }
}

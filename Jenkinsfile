@Library("Shared") _
pipeline{
    agent {label "dev"};
    
    stages{
        stage("Code checkout"){
            steps{
                script {
                    clone("https://github.com/shreeaws/two-tier-flask-app.git", "master")
                }
            }
        }
        stage("Trivy system scan report"){
            steps{
                script{
                    trivy_fs()
                }
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
                        credentialsId:"dockerHubId",
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
post {
    success {
        script {
        emailext body: 'Build Successful',
                 subject: 'Build successful',
                 to: 'shreetatte06@gmail.com'
        }
    }
    failure{
        script {
        emailext body: 'Build failed',
                 subject: 'Build failed',
                 to: 'shreetatte06@gmail.com'
        }
    }
}
}

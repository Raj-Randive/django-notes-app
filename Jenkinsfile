pipeline{
    agent any
    
    stages{
        stage("Clone Code"){
            steps{
                echo "Cloning the Code"
                git url:"https://github.com/Raj-Randive/django-notes-app.git", branch: "main"
            }
        }
        stage("Build"){
            steps{
                echo "Building the image."
                sh "docker build -t my-note-app ."
            }
        }
        stage("Push to DockerHub"){
            steps{
                echo "Pushing the image to DockerHub."
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]){
                    sh "docker tag my-note-app ${env.dockerHubUser}/my-note-app:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/my-note-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploying the container."
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}

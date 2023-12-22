pipeline {
    agent any
    stages {
        stage("Cloning") {
            steps {
                echo "Cloning the code"
                git url:"https://github.com/priyanshu8890/django-notes-app.git", branch: "main"
            }
        }
        stage("Build") {
            steps {
                echo "Building the image"
                sh "docker build -t notes-app ."
            }
        }
        stage("Push") {
            steps {
                echo "Pushing the image to dockerhub"
                withCredentials([usernamePassword(credentialsId:"dockerhub",usernameVariable:"dockerhubUser",passwordVariable:"dockerhubPass")]){
                sh "docker tag notes-app ${env.dockerhubUser}/notes-app:latest"
                sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPass}"
                sh "docker push ${env.dockerhubUser}/notes-app:latest"
                }
            }
        }
        stage("Deploy") {
            steps {
                echo "Deploying the app"
                withCredentials([usernamePassword(credentialsId:"dockerhub",usernameVariable:"dockerhubUser",passwordVariable:"dockerhubPass")]){
                sh "docker-compose down && docker-compose up -d"
                }
            }
        }
    }
}

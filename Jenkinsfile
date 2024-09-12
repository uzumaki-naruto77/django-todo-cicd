pipeline {
    agent {label 'dev-agent'} 
    
    stages{
        stage("Check which user is running this Job"){
            steps{
                sh 'whoami'
            }
        }
        stage("Clone Code"){
            steps {
                echo "Cloning the code"
                git url:"https://github.com/ABHAY4321/django-todo-cicd.git", branch: "develop"
            }
        }
        stage("Build"){
            steps {
                echo "Building the image"
                sh "docker build -t django-todo-app:v1 ."
            }
        }
        stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag django-todo-app:v1 ${env.dockerHubUser}/django-todo-app:v1"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/django-todo-app:v1"
                }
            }
        }
        stage("Deploy"){
            steps {
                echo "Deploying the container on dev server"
                sh "docker-compose down && docker-compose up -d"
                
            }
        }
    }
}

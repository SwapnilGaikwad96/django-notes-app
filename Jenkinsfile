pipeline{
    agent any
        stages{
            stage("Clone Code"){
                steps{
                    echo "Cloning the code"
                    git url: "https://github.com/SwapnilGaikwad96/django-notes-app.git", branch: "main"
                }
            }
            stage("Build image"){
                steps{
                    echo "Building the image"
                    sh "docker build -t myapp ."
                }
            }
            stage("Push to Docker Hub"){
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag myapp ${env.dockerHubUser}/myapp:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/myapp:latest"
                }
            }
        }
            stage("Deploy app"){
                steps{
                    echo "Deploying the app"
                    sh "docker container run -d -p 8000:8000 myapp:latest"
                }
            }
        }
}

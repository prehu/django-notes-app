pipeline{
    agent any
    
    stages{
        stage("code"){
            steps{
                echo "cloning the code"
                git url:"https://github.com/prehu/django-notes-app.git" , branch:"main" 
            }
        }
        stage("build"){
            steps{
                echo "building an image"
                sh "docker build -t mynotesapp ."
            }
        }
        stage("push to docker hub"){
            steps{
                echo "pushing image to docker hub"
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
        	    sh "docker tag mynotesapp ${env.dockerHubUser}/mynotesapp:latest"
        	    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
        	    sh "docker push ${env.dockerHubUser}/mynotesapp:latest"
                }
            }
        }
        stage("deploy"){
            steps{
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}

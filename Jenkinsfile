pipeline {
    agent any
    stages {
        stage("Clone") {
            steps {
                echo "Cloning Git Code"
                git url:"https://github.com/spelluri/nodejs-app.git",branch:"main"
            }
        }
        stage("build") {
            steps {
                echo "docker build image"
                sh "docker build -t my-nodejs-app ."
            }
        }
        stage("login and push") {
            steps {
                echo "dockerhub login and pushing docker image to dockerhub"
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubPass",usernameVariable:"dockerhubUser")]){
                    echo "logging in to dockerhub"
                    sh "docker tag my-nodejs-app ${env.dockerhubUser}/my-nodejs-app:$BUILD_NUMBER"
                    sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPass}"
                    sh "docker push ${env.dockerhubUser}/my-nodejs-app:$BUILD_NUMBER"
                }
            }
        }
        stage("deploy") {
            steps {
                echo "deploying container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}

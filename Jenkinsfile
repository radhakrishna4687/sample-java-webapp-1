pipeline {
    agent any 
    environment {
        //DOCKER_TAG = getDockerTag()
        Docker_image = docker-test-server-1
        Container_name = server-test-1
        Docker_repo= radhakrishna4687
        Github_branch = sample-java-webapp-1
    }
    stages {
        stage ('Clone Repository') {
            steps {
                echo "Checkout Git Repo"
                git credentialsId: 'github', url: 'https://github.com/radhakrishna4687/java-servlet-login-and-display'
               // gitbranch: "master", url 'https://github.com/radhakrishna4687/${Github_branch}.git'
                echo "Complted the Checkout the Git"
            }

        }
        stage ('Build the Dockerfie to create  Docker Image') {
            steps {
                sh '''
                    docker build -t ${Docker_image} -f Dockerfile .
                    docker images
                ''' 
                //sh "docker build .  -t radhakrishna4687/test-image-1:${DOCKER_TAG} "
            }
        }
        stage ('Run the Docker Container') {
            steps {
                sh '''
                    docker run -itd --name=${Container_name} -p 8091:8080 ${Docker_image}
                    docker ps 
                    docker inspect ${Container_name}
                '''    
            }
        }
        stage ('Docker image push to Dockerhub Repository') {
            steps {
                sh 'docker tag ${Container_name} ${Docker_repo}/${Docker_image}:0.0.1'
            }
        }
        stage ('DockerHub Push') {
            steps{ 
                withCre
            }
        }
    }
}

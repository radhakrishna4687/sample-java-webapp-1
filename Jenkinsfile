pipeline {
    agent any 
    environment {
        DOCKER_TAG = getDockerTag()
        DOCKER_IMAGE = "docker-test-server-1"
        CONTAINER_NAME = "server-test-1"
        GITHUB_BRANCH = "java-servlet-login-and-display"
       // DOCKER_REPO = "radhakrishna4687"
        registryCredential = ‘dockerhub’
    }
    stages {
        stage ('Clone Repository') {
            steps {
                echo "Checkout Git Repo"
                git credentialsId: 'github', url: 'https://github.com/radhakrishna4687/:${GITHUB_BRANCH}'
               // gitbranch: "master", url 'https://github.com/radhakrishna4687/${Github_branch}.git'
                echo "Complted the Checkout the Git"
            }

        }
        stage ('Build Docker Image') {
            steps {
                sh '''
                    docker build . -t ${DOCKER_IMAGE} -f "Dockerfile "
                    docker images
                ''' 
                //sh "docker build .  -t radhakrishna4687/test-image-1:${DOCKER_TAG} "
            }
        }
        stage ('Run the Docker Container') {
            steps {
                sh '''
                    docker run -itd --name=${CONTAINER_NAME} -p 8091:8080 ${DOCKER_IMAGE}
                    docker ps 
                    docker inspect ${Container_name}
                '''    
            }
        }
        stage ('Docker image push to Dockerhub Repository') {
            steps {
                sh 'docker tag ${CONTAINER_NAME} radhakrishna4687/${DOCKER_IMAGE}:${DOCKER_TAG}'
            }
        }
        stage ('Push docker image to DockerHub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhubpwd')]) {
                    sh "docker login -u radhakrishna4687 -p ${dockerhubpwd}"
                    sh "docker push radhakrishna4687/${DOCKER_IMAGE}:${DOCKER_TAG}"
                }
            }
        }
    }
}
//Dynamically allocate the version name with latest commit id
def getDockerTag() {
    def tag = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}

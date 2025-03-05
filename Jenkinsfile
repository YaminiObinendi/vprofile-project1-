pipeline {
    agent {label 'Node1'}

    environment {
        DOCKER_IMAGE_NAME = "yaminiobinendi/vprofile"  
        DOCKER_CREDENTIALS = "dockerhub-credentials" 
        
    }

    stages {
        stage('SCM Checkout') {
            steps {
                script {
                    git branch: 'docker', url: 'https://github.com/YaminiObinendi/vprofile-project1-.git'
                }
            }
        }

        stage('Maven Build') {
            steps {
                script {
                    // Run Maven build
                    sh "mvn clean install -Dversion=${BUILD_NUMBER}"
                }
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    def artifactPath = "/home/ubuntu/.m2/repository/com/visualpathit/vprofile/v2/vprofile-v2.war"

                    // Copy the artifact into the Docker context (e.g., tomcat webapps directory)
                    sh """
                    mkdir -p ./webapps
                    cp ${artifactPath} ./webapps/
                    """

                    // Build the Docker image with the build number as the tag
                    sh """
                    docker build -t ${DOCKER_IMAGE_NAME}:${BUILD_NUMBER} .
                    """

                    // Login to Docker Hub
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS, passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                        sh """
                        docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD} 
                        """

                        // Push the Docker image to Docker Hub
                        sh """
                        docker push ${DOCKER_IMAGE_NAME}:${BUILD_NUMBER}
                        """
                    }
                }
            }
        }
    }
}

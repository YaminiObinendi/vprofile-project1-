pipeline {
    agent any

    environment {
        TOMCAT_SERVER = "51.21.201.144" // Tomcat server address
        TOMCAT_WEBAPPS_DIR = "/opt/tomcat9/webapps" // Replace with your Tomcat webapps directory path
        ARTIFACT_NAME = "vprofile-v2.war" // Name of your artifact (adjust as necessary)
    }

    stages {
        // Stage 1: SCM Checkout
        stage('SCM Checkout') {
            steps {
                script {
                    // Checkout the code from the Git repository
                    git branch: 'jenkins', url: 'https://github.com/YaminiObinendi/vprofile-project1-.git'
                }
            }
        }

        // Stage 2: Maven Build
        stage('Maven Build') {
            steps {
                script {
                    // Run Maven build and generate the artifact, using the Jenkins build number as the version
                    sh "mvn clean install  -Dversion=${env.BUILD_ID}"
                    
                    // Archive the artifact  for later stages if needed
                    archiveArtifacts artifacts: "/home/ubuntu/workspace/Tomcat-Job/target/${ARTIFACT_NAME}", allowEmptyArchive: true
                }
            }
        }

        // Stage 3: Deploy to Tomcat
        stage('Deploy') {
            steps {
                script {
                    def artifactPath = "/home/ubuntu/workspace/Tomcat-Job/target/${ARTIFACT_NAME}"

                    if (fileExists(artifactPath)) {
                        // Deploy the WAR file to Tomcat webapps directory
                        sh "cp ${artifactPath} ${TOMCAT_WEBAPPS_DIR}/"
                        
                        
                    } else {
                        error "Artifact not found: ${artifactPath}"
                    }
                }
            }
        }
    }
}

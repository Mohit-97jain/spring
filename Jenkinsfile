pipeline{
    agent any
    environment {
        DOCKER_IMAGE = "mj36172/pipeline"   
    }
    stages{
        stage("init"){
            steps{
           echo "initializeing the pipeline" 
        }
        }
       stage("checkout"){
            steps {
                git branch: 'master', url: 'https://github.com/Mohit-97jain/spring.git'
            }
        }
        stage('Build JAR') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }
        stage('Build Docker Image') {
            steps {
                bat "docker build -t ${DOCKER_IMAGE}:${env.BUILD_NUMBER} ."
            }
        }

        stage ('docker push'){
            steps{
                withDockerRegistry([credentialsId: 'dockerhub-creds', url: 'https://index.docker.io/v1/']) {
                    bat "docker push ${DOCKER_IMAGE}:${env.BUILD_NUMBER}"
                    bat "docker tag ${DOCKER_IMAGE}:${env.BUILD_NUMBER} ${DOCKER_IMAGE}:latest"
                    bat "docker push ${DOCKER_IMAGE}:latest"
            }
        }


        }

    }


post {
        success {
            echo "✅ Build and Push successful!"
        }
        failure {
            echo "❌ Build or Push failed!"
        }
    }
}

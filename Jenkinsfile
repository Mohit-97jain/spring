pipeline{
    agent any
    environment {
        DOCKER_IMAGE = "mj36172/pipeline"   
    }
     tools {
        maven 'Maven'   
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

        stage('Push to Docker Hub') {
    steps {
        script {
            docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-creds') {
                def app = docker.build("${DOCKER_IMAGE}:${env.BUILD_NUMBER}")
                app.push()
                app.push("latest")
            }
        }
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

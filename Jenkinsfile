pipeline {
    agent any

    environment {
        IMAGE_Name = 'neeravnilay/cicd_demo:latest'
    }

    stages {
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    script {
                        def scannerHome = tool name: 'SonarScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=cicd_demo_project -Dsonar.sources=. -Dsonar.host.url=$SONARQUBE_URL"
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_Name .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withDockerRegistry(credentialsId: 'docker-hub-credentials', url: '') {
                        sh "docker push $IMAGE_Name"
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh "docker run -d -p 5000:5000 $IMAGE_Name"
            }
        }
    }
}

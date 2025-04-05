pipeline{
    agent any
    environment{
        IMAGE_Name = 'neeravnilay/cicd_demo:latest'
<<<<<<< HEAD
=======
        SONARQUBE_URL = 'http://localhost:9000'
>>>>>>> 49bd535 (second commit)
    }

    stages{
        stage('clone Repository'){
            steps{
                git https://github.com/manuneerav/cicd_demo.git
            }


        }

        stage('Sonarcube Analysis'){
            steps{
                withSonarQubeEnv('SonarQube') {
                    sh 'sonar-scanner -Dsonar.projectKey=cicd_demo_project -Dsonar.sources=. -Dsonar.host.url=$SONARQUBE_URL'
                }
            }

        }


        stage('build Image'){
            steps{
                docker build -t $IMAGE_Name .
            }
        }

        stage("Push Image"){
            steps{
                docker.withDockerRegistry(credentialsId: 'docker-hub-credentials', url: '') {
                    sh "docker push $IMAGE_Name"
                }
            }
        }

        stage('Deploy'){
            steps{
                sh "docker run -d -p 5000:5000 $IMAGE_Name"
            }
        }
    }
}

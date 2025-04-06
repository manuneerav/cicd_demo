pipeline{
    agent any
    environment{
        IMAGE_Name = 'neeravnilay/cicd_demo:latest'

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
                sh 'docker build -t $IMAGE_Name .'
            }
        }

        stage("Push Image"){
            steps{
                script{
                    docker.withDockerRegistry(credentialsId: 'docker-hub-credentials', url: '') {
                    sh "docker push $IMAGE_Name"
                }
            }
                
        }
    }

        stage('Deploy'){
            steps{
                sh "docker run -d -p 5000:5000 $IMAGE_Name"
            }
        }
    }


pipeline { 

    environment { 

        registry = "350557/apache" 

        registryCredential = 'dockerhub'

        dockerImage = '' 

    }

    agent any 

    stages { 

        stage('Cloning our Git') { 

            steps { 

                git branch: 'main', credentialsId: 'git', url: 'https://github.com/beeralas/apache.git'

            }

        } 

        stage('Building our image') { 

            steps { 

                script { 
                    dockerImage = docker.build registry + ":$BUILD_NUMBER" 
                }
            } 
        }
        stage('Deploy our image') { 
            steps { 
                script { 
                    docker.withRegistry( '', registryCredential ) { 
                        dockerImage.push() 
                    }
                } 
            }
        } 
        stage('Cleaning up') { 
            steps { 
                sh "docker rmi $registry:$BUILD_NUMBER" 
            }
        } 
    }
}

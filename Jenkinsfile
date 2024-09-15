pipeline {
    agent any

    options {
        timeout(time: 2, unit: 'MINUTES')
    }

    environment {
        ARTIFACT_ID = "dlavezzari/webapp:${env.BUILD_NUMBER}"
    }

    stages {
        stage('Building image') {
            steps {
                script {
                    // Autenticarse en Docker Hub usando las credenciales configuradas
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-pipe') {
                        sh '''
                        docker build -t testapp .
                        '''
                    }
                }
            }
        }

        stage('Run tests') {
            steps {
                sh "docker run testapp npm test"
            }
        }

        stage('Deploy Image') {
            steps {
                script {
                    // Definir la variable BUILD_NUMBER
                    def buildNumber = env.BUILD_NUMBER
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-pipe') {
                        sh """
                        docker tag testapp dlavezzari/webapp:${buildNumber}
                        docker push dlavezzari/webapp:${buildNumber}
                        """
                    }
                }
            }
        }
    }
}

    
  


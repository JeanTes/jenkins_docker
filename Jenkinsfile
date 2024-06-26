pipeline {
    environment {
        IMAGEN = "JeanTes/myapp"
        USUARIO = 'USER_DOCKERHUB'
    }
    agent any
    stages {
        stage('Clone') {
            steps {
                git branch: "main", url: 'https://github.com/JeanTes/jenkins_docker.git'
                echo "Se clona"
            }
        }
        stage('Build') {
            steps {
                script {
                    newApp = docker.build "$IMAGEN:$BUILD_NUMBER"
                    echo "Se buildea aka falsea"
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    docker.image("$IMAGEN:$BUILD_NUMBER").inside('-u root') {
                           sh 'apache2ctl -v'
                           echo "Se testea"
                        }
                    }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    docker.withRegistry( '', USUARIO ) {
                        newApp.push()
                        echo "Se publica"
                    }
                }
            }
        }        
    }
}

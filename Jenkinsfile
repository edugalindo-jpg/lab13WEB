pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                    # Liberar el puerto 8081 en caso de que esté ocupado por un despliegue anterior
                    fuser -k 8081/tcp || true
                    sleep 2

                    # Lanzar la aplicación en segundo plano en el puerto 8081
                    BUILD_ID=dontKillMe nohup java -jar target/*.jar --server.port=8081 > app.log 2>&1 &
                '''
            }
        }
    }
}

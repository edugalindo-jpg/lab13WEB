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
                    # Detener cualquier proceso previo del JAR para liberar el puerto
                    pkill -f '.*\\.jar' || true

                    # Lanzar el nuevo ejecutable en segundo plano
                    BUILD_ID=dontKillMe nohup java -jar target/*.jar > app.log 2>&1 &
                '''
            }
        }
    }
}

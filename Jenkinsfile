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
                    fuser -k 8081/tcp || true
                    sleep 3
                    BUILD_ID=dontKillMe setsid nohup java -jar target/*.jar --server.port=8081 > /tmp/petclinic.log 2>&1 < /dev/null &
                    sleep 5
                '''
            }
        }
    }
}

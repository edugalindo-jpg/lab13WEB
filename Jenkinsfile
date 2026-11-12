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
                    pkill -f "petclinic.*jar" || true
                    sleep 3
                    JENKINS_NODE_COOKIE=dontKillMe BUILD_ID=dontKillMe setsid nohup java -jar target/*.jar --server.port=8081 --spring.h2.console.settings.web-allow-others=true > /tmp/petclinic.log 2>&1 </dev/null &
                    sleep 8
                    tail -n 15 /tmp/petclinic.log
                '''
            }
        }
    }
}

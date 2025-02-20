pipeline {
    agent any

    triggers {
        cron('H/10 * * * 1') // Runs every 10 minutes on Mondays
    }

    environment {
        JDK_VERSION = '11'
        MAVEN_OPTS = '-Dmaven.test.failure.ignore=true'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Khyati1997/spring-petclinic-ci.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Run Tests & Generate Code Coverage') {
            steps {
                script {
                    sh 'mvn test'
                    sh 'mvn jacoco:report'
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

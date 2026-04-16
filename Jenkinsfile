pipeline {
    agent any

    tools {
        maven 'Maven'   // Must match Jenkins Global Tool Configuration
        jdk 'JDK'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/thakurkatawa/MyMavenSeleniumApp02.git'
            }
        }

        stage('Build (Compile)') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Run Selenium Tests') {
            steps {
                // Allow pipeline to continue even if tests fail
                sh 'mvn test -Dmaven.test.failure.ignore=true'
            }
        }

        stage('Package') {
            steps {
                // Skip tests here to avoid running twice
                sh 'mvn package -DskipTests'
            }
        }
    }

    post {
        always {
            // Publish test reports
            junit '**/target/surefire-reports/*.xml'
        }

        success {
            echo 'Build and Selenium tests executed successfully!'
        }

        unstable {
            echo 'Some tests failed. Build is unstable.'
        }

        failure {
            echo 'Build failed due to compilation or configuration errors.'
        }
    }
}

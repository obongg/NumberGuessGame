pipeline {
    agent any

    tools {
        maven 'Maven'    
        jdk 'jdk17'  
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/obongg/NumberGuessGame.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh '''
                    echo "Stopping Tomcat..."
                    /home/ec2-user/tomcat10/bin/shutdown.sh || true

                    echo "Cleaning old deployment..."
                    rm -rf /home/ec2-user/tomcat10/webapps/ROOT*

                    echo "Deploying new WAR..."
                    cp target/*.war /home/ec2-user/tomcat10/webapps/ROOT.war

                    echo "Starting Tomcat..."
                    /home/ec2-user/tomcat10/bin/startup.sh
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Build, Test, and Deployment completed successfully!'
        }
        failure {
            echo '❌ Something went wrong. Check logs for errors.'
        }
    }
}


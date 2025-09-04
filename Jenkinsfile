pipeline {
    agent any

<<<<<<< HEAD
    environment {
        JAVA_HOME = tool name: 'jdk17', type: 'jdk'
        MAVEN_HOME = tool name: 'Maven', type: 'maven'
        CATALINA_HOME = '/home/ec2-user/tomcat10'
=======
    tools {
        maven 'Maven'    
        jdk 'jdk17'  
>>>>>>> 26ff3363985e824b1d565935c5699732c9d8c026
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/obongg/NumberGuessGame.git'
            }
        }

        stage('Build') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn clean package -DskipTests"
            }
        }

        stage('Test') {
            steps {
                sh "${MAVEN_HOME}/bin/mvn test"
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    // Give Jenkins user ownership of Tomcat directories
                    sh "sudo chown -R jenkins:jenkins $CATALINA_HOME"
                    sh "sudo chmod -R 755 $CATALINA_HOME"

                    echo "Stopping Tomcat..."
                    sh "$CATALINA_HOME/bin/shutdown.sh || true"
                    sleep 5

                    echo "Cleaning old deployment..."
                    sh "rm -rf $CATALINA_HOME/webapps/ROOT*"

                    echo "Deploying new WAR..."
                    sh "cp target/*.war $CATALINA_HOME/webapps/ROOT.war"

                    echo "Starting Tomcat..."
                    sh "$CATALINA_HOME/bin/startup.sh"
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed. Check the logs."
        }
    }
}


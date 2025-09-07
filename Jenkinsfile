pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/obongg/NumberGuessGame.git', branch: 'main', credentialsId: '8688c497-760e-4259-8c37-cbfe8ad065f8'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Code Quality - SonarQube') {
            steps {
                withSonarQubeEnv('MySonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Run with Jetty') {
            steps {
                sh 'mvn jetty:run &'
                sh 'sleep 10'
                echo 'Application deployed on Jetty!'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully ✅'
            emailext(
                subject: "✅ SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """<p>Good news!</p>
                         <p>Build <b>${env.BUILD_NUMBER}</b> succeeded.</p>
                         <p>Check details: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>""",
                to: 'esthermonday3@gmail.com',
                recipientProviders: [[$class: 'CulpritsRecipientProvider'], [$class: 'DevelopersRecipientProvider']]
            )
        }
        failure {
            echo 'Pipeline failed ❌'
            emailext(
                subject: "❌ FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """<p>Oops!</p>
                         <p>Build <b>${env.BUILD_NUMBER}</b> failed.</p>
                         <p>Check logs: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>""",
                to: 'esthermonday3@gmail.com',
                recipientProviders: [[$class: 'CulpritsRecipientProvider'], [$class: 'DevelopersRecipientProvider']]
            )
        }
        always {
            script {
                echo "📧 Email notification attempted via Extended Email plugin"
            }
        }
    }
}

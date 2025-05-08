
pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo "👋 This is branch: ${env.BRANCH_NAME}"
            }
        }
    }

    post {
        success {
            slackSend channel: '#welcome', message: "✅ Build SUCCESS on ${env.BRANCH_NAME}"
        }
        failure {
            slackSend channel: '#welcome', message: "❌ Build FAILED on ${env.BRANCH_NAME}"
        }
    }
}

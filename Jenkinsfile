pipeline {
    agent any

    triggers {
        githubPush()
    }

    environment {
        GITHUB_TOKEN = credentials('github-token')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Gather Commit Info') {
            steps {
                script {
                    def commitMessage = sh(script: "git log -1 --pretty=%B", returnStdout: true).trim()
                    def author = sh(script: "git log -1 --pretty=format:'%an'", returnStdout: true).trim()
                    def commitHash = sh(script: "git rev-parse --short HEAD", returnStdout: true).trim()

                    env.LAST_COMMIT_MESSAGE = commitMessage
                    env.LAST_COMMIT_AUTHOR = author
                    env.LAST_COMMIT_HASH = commitHash
                }
            }
        }

        stage('Build') {
            steps {
                echo "📦 Building ${env.BRANCH_NAME}"
                sleep 2
            }
        }

        stage('Test') {
            steps {
                echo "🧪 Testing ${env.BRANCH_NAME}"
                sleep 2
            }
        }

        stage('Deploy') {
            steps {
                echo "🚀 Deploying ${env.BRANCH_NAME}"
                sleep 2
            }
        }
    }

    post {
        success {
            slackSend channel: '#welcome',
                message: """✅ Build SUCCESS on *${env.BRANCH_NAME}* by *${env.LAST_COMMIT_AUTHOR}*
📝 ${env.LAST_COMMIT_MESSAGE} (${env.LAST_COMMIT_HASH})"""

            emailext subject: "Build SUCCESS on ${env.BRANCH_NAME}",
                     body: """Branch: ${env.BRANCH_NAME}
Author: ${env.LAST_COMMIT_AUTHOR}
Commit: ${env.LAST_COMMIT_MESSAGE} (${env.LAST_COMMIT_HASH})""",
                     to: "aaronvelezcoronado@gmail.com"
        }

        failure {
            slackSend channel: '#welcome',
                message: """❌ Build FAILED on *${env.BRANCH_NAME}* by *${env.LAST_COMMIT_AUTHOR}*
📝 ${env.LAST_COMMIT_MESSAGE} (${env.LAST_COMMIT_HASH})"""

            emailext subject: "Build FAILED on ${env.BRANCH_NAME}",
                     body: """Branch: ${env.BRANCH_NAME}
Author: ${env.LAST_COMMIT_AUTHOR}
Commit: ${env.LAST_COMMIT_MESSAGE} (${env.LAST_COMMIT_HASH})""",
                     to: "aaronvelezcoronado@gmail.com"
        }
    }
}

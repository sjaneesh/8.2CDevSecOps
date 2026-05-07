pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/sjaneesh/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0'
            }
            post {
                always {
                    emailext(
                        to: 'aneesh23usa@gmail.com',
                        subject: "Test Stage - ${currentBuild.currentResult}: Job ${env.JOB_NAME} Build ${env.BUILD_NUMBER}",
                        body: """
                            <p>The <b>Run Tests</b> stage has completed with status: <b>${currentBuild.currentResult}</b></p>
                            <p>Job: ${env.JOB_NAME}</p>
                            <p>Build Number: ${env.BUILD_NUMBER}</p>
                            <p>Check console output at: <a href='${env.BUILD_URL}'>${env.BUILD_URL}</a></p>
                        """,
                        mimeType: 'text/html',
                        attachLog: true
                    )
                }
            }
        }

        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }
            post {
                always {
                    emailext(
                        to: 'aneesh23usa@gmail.com',
                        subject: "Security Scan Stage - ${currentBuild.currentResult}: Job ${env.JOB_NAME} Build ${env.BUILD_NUMBER}",
                        body: """
                            <p>The <b>NPM Audit (Security Scan)</b> stage has completed with status: <b>${currentBuild.currentResult}</b></p>
                            <p>Job: ${env.JOB_NAME}</p>
                            <p>Build Number: ${env.BUILD_NUMBER}</p>
                            <p>Check console output at: <a href='${env.BUILD_URL}'>${env.BUILD_URL}</a></p>
                        """,
                        mimeType: 'text/html',
                        attachLog: true
                    )
                }
            }
        }

    }
}

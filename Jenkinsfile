pipeline {
    agent any

    environment {
        DEPLOY_DIR = 'C:\\Deploy\\JenkinsDemo20'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Run Script') {
            steps {
                bat 'script.bat'
            }
        }

        stage('Generate HTML Report') {
            steps {
                bat '''
                if not exist report mkdir report
                (
                echo ^<html^>
                echo ^<head^>^<title^>Jenkins Report^</title^>^</head^>
                echo ^<body^>
                echo ^<h1^>Jenkins Build Report^</h1^>
                echo ^<p^>Build ran successfully.^</p^>
                echo ^</body^>
                echo ^</html^>
                ) > report\\report.html
                '''
            }
        }

        stage('Deploy to Local Folder') {
            steps {
                bat '''
                if not exist "%DEPLOY_DIR%" mkdir "%DEPLOY_DIR%"
                copy report\\report.html "%DEPLOY_DIR%\\report.html" /Y
                '''
            }
        }
    }

    post {
        success {
            emailext(
                subject: "Jenkins Build SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Build succeeded. The HTML report was deployed to ${env.DEPLOY_DIR}.",
                to: "YOUR_EMAIL@gmail.com"
            )
        }
        failure {
            emailext(
                subject: "Jenkins Build FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Build failed. Please check the Jenkins console output.",
                to: "YOUR_EMAIL@gmail.com"
            )
        }
    }
}
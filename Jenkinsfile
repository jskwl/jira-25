// Manage Jenkins → Configure System → Jira (or “Jira Steps”)
// Add a Jira Site:
// Server URL: your Jira base URL (e.g. https://yourcompany.atlassian.net)
// Credentials: select jira-jenkin
// Create Credentials as Globale username - password

pipeline {
    agent any

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage('Check Git pull') {
            steps {
                sh 'ls -l'
            }
        }

        stage('Build') {
            steps {
                echo 'Building..'
                echo "${env.JOB_NAME}"
                echo "${env.BUILD_NUMBER}"
            }
        }

      // stage JiraUpdate

    }
}

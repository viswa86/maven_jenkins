pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                // Checkout the repository using SCM
                checkout scm
            }
        }
        stage('Deploy Code') {
            steps {
                // Use the SSH key for the server
                sshagent(['SSH_KEY']) {
                    script {
                        // Run commands on the remote server
                        sh '''
                        ssh -o StrictHostKeyChecking=no ubuntu@52.66.64.59 <<EOF
                        set -e  # Stop execution if any command fails
                        cd /home/ubuntu/project/maven_jenkins
                        git fetch origin staging
                        git reset --hard origin/staging
                        git checkout staging
                        git status
                        EOF
                        '''
                    }
                }
            }
        }
    }
}

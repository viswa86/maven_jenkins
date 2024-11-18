pipeline {
    agent any
    environment {
        // Replace 'SONARCLOUD_TOKEN' with the credential ID
        SONAR_TOKEN = credentials('31dbb0d9479abbaba6a1629b1f2a81a09e00ae0b')
    }
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        stage('SonarCloud Analysis') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh '''
                    mvn clean verify sonar:sonar \
                      -Dsonar.projectKey=viswa86 \
                      -Dsonar.organization=viswa86 \
                      -Dsonar.host.url=https://sonarcloud.io \
                      -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }
        stage('Quality Gate') {
            steps {
                script {
                    timeout(time: 2, unit: 'MINUTES') {
                        def qualityGate = waitForQualityGate()
                        if (qualityGate.status != 'OK') {
                            echo "WARNING: Quality Gate failed. Status: ${qualityGate.status}"
                        } else {
                            echo "Quality Gate passed successfully."
                        }
                    }
                }
            }
        }
        stage('Deploy Code') {
    steps {
        // Use the SSH key for the server
        sshagent(['SSH_KEY']) {
            script {
                // Run commands on the remote server
                sh '''
                ssh -o StrictHostKeyChecking=no ubuntu@15.207.21.74 <<EOF
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

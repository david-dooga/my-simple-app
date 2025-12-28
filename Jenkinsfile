pipeline {
    agent any
    environment {
        // Change this to your actual Docker Hub repository
        DOCKER_HUB_REPO = 'doogadavid/my-simple-app'
        // Uses the Jenkins build number as a version tag
        IMAGE_TAG = "v${env.BUILD_NUMBER}"
    }
    stages {
        stage('Checkout Source Code') {
            steps {
                // Pulls the latest code (including Dockerfile and Playbook) from Git
                checkout scm
            }
        }
        stage('Run Ansible Build & Push') {
            steps {
                // securely retrieves Docker Hub credentials from Jenkins store
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', 
                                                  passwordVariable: 'D_PASS', 
                                                  usernameVariable: 'D_USER')]) {
                    
                    // Executes the playbook defined above
                    ansiblePlaybook(
                        playbook: 'build-push.yml',
                        inventory: '/etc/ansible/hosts',
                        extraVars: [
                            docker_repo: "${DOCKER_HUB_REPO}",
                            tag: "${IMAGE_TAG}",
                            d_user: "${D_USER}",
                            d_pass: "${D_PASS}"
                        ]
                    )
                }
            }
        }
    }
    post {
        success {
            echo "Build successful! Image pushed to ${DOCKER_HUB_REPO}:${IMAGE_TAG}"
        }
        failure {
            echo "Pipeline failed. Check the Ansible logs in Jenkins Console Output."
        }
    }
}


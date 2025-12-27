pipeline {
    agent any
    environment {
        // IDs must match the Credentials you created in the Jenkins UI
        GITHUB_CREDS_ID  = 'github-ssh-key' 
        DOCKER_HUB_CREDS = 'dockerhub-creds'
        SSH_KEY_ID       = 'managed-node-ssh' // For deployment instance
        
        IMAGE_NAME       = "doogadavid/my-app"
        REMOTE_NODE_IP   = "44.199.242.0"
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Clones the repository using the SSH key
                checkout scmGit(
                    branches: [[name: '*/main']], 
                    userRemoteConfigs: [[url: 'git@github.com:david-dooga/my-simple-app.git', 
                                         credentialsId: "${GITHUB_CREDS_ID}"]]
                )
            }
        }

        stage('Build & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKER_HUB_CREDS}", 
                                 passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                    sh "ansible-playbook build_image.yml -e 'docker_user=${DOCKER_USER} docker_pass=${DOCKER_PASS} img_name=${IMAGE_NAME} img_tag=${BUILD_NUMBER}'"
                }
            }
        }

        stage('Deploy') {
            steps {
                sshagent([SSH_KEY_ID]) {
                    sh "ansible-playbook -i ${REMOTE_NODE_IP}, deploy_app.yml -u ec2-user -e 'img_name=${IMAGE_NAME} img_tag=${BUILD_NUMBER}'"
                }
            }
        }
    }
}


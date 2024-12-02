pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "applebite/php-app"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/rrjoshi9/projCert.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t ${DOCKER_IMAGE}:latest .'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh '''
                    docker login -u rrjoshi9 -p Rahul@12345
                    docker tag ${DOCKER_IMAGE}:latest rrjoshi9/php-app:latest
                    docker push rrjoshi9/php-app:latest
                    '''
                }
            }
        }

        stage('Deploy to Dev') {
    steps {
        script {
            echo "Current branch: ${env.BRANCH_NAME}"  // Print the branch name
            if (env.BRANCH_NAME == 'dev') {
                sh 'ansible-playbook -i ansible/hosts ansible/deploy.yml --extra-vars "env=dev"'
            } else {
                echo "Skipping deployment as not on the dev branch"
            }
        }
    }
}

        stage('Deploy to Stage') {
    steps {
        script {
            echo "Current branch: ${env.BRANCH_NAME}"  // Print the branch name
            if (env.BRANCH_NAME == 'stage') {
                sh 'ansible-playbook -i ansible/hosts ansible/deploy.yml --extra-vars "env=stage"'
            } else {
                echo "Skipping deployment as not on the dev branch"
            }
        }
    }
}

        stage('Deploy to Prod') {
    steps {
        script {
            echo "Current branch: ${env.BRANCH_NAME}"  // Print the branch name
            if (env.BRANCH_NAME == 'prod') {
                sh 'ansible-playbook -i ansible/hosts ansible/deploy.yml --extra-vars "env=prod"'
            } else {
                echo "Skipping deployment as not on the dev branch"
            }
        }
    }
}

    post {
        always {
            echo "CI/CD Pipeline Completed!"
        }
    }
}

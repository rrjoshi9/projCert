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
            when {
                branch 'dev'
            }
            steps {
                script {
                    sh 'ansible-playbook -i ansible/hosts ansible/deploy.yml --extra-vars "env=dev"'
                }
            }
        }

        stage('Deploy to Stage') {
            when {
                branch 'stage'
            }
            steps {
                script {
                    sh 'ansible-playbook -i ansible/hosts ansible/deploy.yml --extra-vars "env=stage"'
                }
            }
        }

        stage('Deploy to Prod') {
            when {
                branch 'prod'
            }
            steps {
                script {
                    sh 'ansible-playbook -i ansible/hosts ansible/deploy.yml --extra-vars "env=prod"'
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

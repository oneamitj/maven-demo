pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'wannamit/mavendemo:latest'
        DOCKER_COMPOSE_FILE = 'docker-compose.yml'
        SONARQUBE_SERVER = 'http://sonarqube:9000' // Update to use own sonarqube server
        SONARQUBE_PROJECT_KEY = 'maven-demo' // Update to use own your sonarqube project key
        DOCKER_HUB_CREDENTIALS_ID = 'docker-hub-credentials'
        SSH_CREDENTIALS_ID = 'ssh-credentials' // Jenkins credentials ID for SSH
        TEST_SERVER = '128.11.12.13' // Test server address, update it
        PROD_SERVER = '128.11.12.14' // Production server address, update it
        SSH_USER = 'amit_user' // User of linux/vm you are using as test or prod server
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/oneamitj/maven-demo.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run the tests inside the Docker container
                    sh 'docker run --rm $DOCKER_IMAGE mvn test'
                }
            }
        }

        stage('Code Quality Analysis') {
            steps {
                script {
                    // Run SonarQube analysis
                    sh 'docker run --rm -v $(pwd):/usr/src -v /root/.m2:/root/.m2 sonarsource/sonar-scanner-cli sonar-scanner \
                        -Dsonar.projectKey=$SONARQUBE_PROJECT_KEY \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=$SONARQUBE_SERVER'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // Login to Docker Hub
                    withCredentials([usernamePassword(credentialsId: "$DOCKER_HUB_CREDENTIALS_ID", usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                        sh 'echo $DOCKER_HUB_PASSWORD | docker login -u $DOCKER_HUB_USERNAME --password-stdin'
                    }
                    // Push the Docker image to Docker Hub
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Deploy to Test') {
            steps {
                script {
                    // Deploy to test environment using SSH
                    sshagent([SSH_CREDENTIALS_ID]) {
                        sh """
                        ssh -o StrictHostKeyChecking=no $SSH_USER@$TEST_SERVER << EOF
                        docker-compose -f $DOCKER_COMPOSE_FILE down
                        docker-compose -f $DOCKER_COMPOSE_FILE pull
                        docker-compose -f $DOCKER_COMPOSE_FILE up -d
                        EOF
                        """
                    }
                }
            }
        }

        stage('Integration Test') {
            steps {
                script {
                    // Run integration tests on the test server
                    sshagent([SSH_CREDENTIALS_ID]) {
                        sh """
                        ssh -o StrictHostKeyChecking=no $SSH_USER@$TEST_SERVER << EOF
                        docker-compose -f $DOCKER_COMPOSE_FILE exec app mvn verify
                        EOF
                        """
                    }
                }
            }
        }

        stage('Deploy to Production') {
            when {
                branch 'master'
            }
            steps {
                script {
                    // Deploy to production environment using SSH
                    sshagent([SSH_CREDENTIALS_ID]) {
                        sh """
                        ssh -o StrictHostKeyChecking=no $SSH_USER@$PROD_SERVER << EOF
                        docker-compose -f $DOCKER_COMPOSE_FILE -f docker-compose.prod.yml down
                        docker-compose -f $DOCKER_COMPOSE_FILE -f docker-compose.prod.yml pull
                        docker-compose -f $DOCKER_COMPOSE_FILE -f docker-compose.prod.yml up -d
                        EOF
                        """
                    }
                }
            }
        }

        stage('Monitoring and Alerting') {
            steps {
                script {
                    // Set up monitoring and alerting
                    sh 'docker run --rm -v $(pwd):/usr/src -v /root/.m2:/root/.m2 datadog/agent:latest'
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }

        failure {
            mail to: 'team@example.com',
                 subject: "Pipeline Failed: ${currentBuild.fullDisplayName}",
                 body: "Something is wrong with ${env.JOB_NAME}.\n\nBuild: ${env.BUILD_URL}\n\nPlease check the logs."
        }
    }
}

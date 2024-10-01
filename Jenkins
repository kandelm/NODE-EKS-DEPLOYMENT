pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                // GitLab repository checkout
                git url: 'https://gitlab.com/your-username/your-repo.git', branch: 'main', credentialsId: 'your-gitlab-credentials'
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Install Node.js dependencies
                    sh 'npm ci'
                }
            }
        }

        stage('Run Unit Tests') {
            steps {
                script {
                    // Run the test suite
                    sh 'npm test'
                }
            }
        }

        stage('Build Application') {
            steps {
                script {
                    // Build the application (if required, like minification or transpiling)
                    sh 'npm run build'
                }
            }
        }

        stage('Code Analysis (SonarQube)') {
            steps {
                script {
                    // SonarQube code analysis
                    withSonarQubeEnv('sonarqube-server') {
                        sh 'sonar-scanner'
                    }
                }
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    // Build Docker image for the Node.js app
                    sh 'docker build -t your-image-name:latest .'
                }
            }
        }

        stage('Deploy to Development') {
            steps {
                script {
                    // Deploy the app to the Kubernetes dev namespace
                    sh 'kubectl apply -f k8s/deployment.yaml -n development'
                }
            }
        }

        stage('Smoke Testing (Development)') {
            steps {
                script {
                    // Run smoke tests to ensure the app is working in the dev environment
                    sh 'curl http://dev-app-url/health-check'
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                script {
                    // Deploy the app to the Kubernetes prod namespace
                    sh 'kubectl apply -f k8s/deployment.yaml -n production'
                }
            }
        }
    }

    post {
        success {
            // Notify on success (email/Slack notification setup)
            echo 'Pipeline completed successfully!'
        }
        failure {
            // Notify on failure
            echo 'Pipeline failed.'
        }
    }
}

pipeline {
    agent {
        kubernetes {
            yamlFile 'kaniko-pod.yaml'
        }
    }
    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        ECR_REPO = '576607007321.dkr.ecr.us-east-1.amazonaws.com/gitops-gp-ecr'
        IMAGE_TAG = "nodejsapp-${env.BUILD_NUMBER}"
        IMAGE_NAME = "$ECR_REPO:$IMAGE_TAG"
    }

    stages {
        stage('Workspace Cleanup') {
            steps {
                cleanWs()
            }
        }

        stage('Clone NodeJS App Repo') {
            steps {
                echo "🔄 Cloning NodeJS application repository..."
                sh 'git clone https://github.com/mahmoud254/jenkins_nodejs_example.git'
                echo "✅ Repository cloned successfully"
            }
        }

        stage('Build & Push Docker Image') {
            steps {
                container('kaniko') {
                    echo "🔨 Building Docker image with Kaniko... & 🚀 Pushing Docker image to ECR..."

                    sh """
                    /kaniko/executor \\
                      --context=./jenkins_nodejs_example \\
                      --dockerfile=./jenkins_nodejs_example/dockerfile \\
                      --destination=$IMAGE_NAME \\
                      --oci-layout-path=/kaniko/oci-layout \\
                      --verbosity=debug \\
                      --cache=true \\
                      --cache-repo=576607007321.dkr.ecr.us-east-1.amazonaws.com/gitops-gp-ecr \\
                      --cache-dir=/kaniko/cache \\
                      --skip-tls-verify=false \\
                    """
                }
                echo "✅ Docker image build complete & pushed : $IMAGE_NAME"
            }
        }
    }

    post {
        failure {
            echo "❌ Pipeline failed. Check above logs for details."
        }
        success {
            echo "🎉 Pipeline completed successfully!"
        }
    }
}

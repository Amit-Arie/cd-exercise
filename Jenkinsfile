pipeline {
    agent any

    environment {
        ECR_REGISTRY = "992382545251.dkr.ecr.us-east-1.amazonaws.com"
        REPO_NAME    = "flask-app" // ודא שזה השם המדויק שנתת ל-Repository ב-ECR
        IMAGE_TAG    = "latest"
    }

    stages {
        // שלב 1: משיכת הקוד המעודכן מ-GitHub
        stage('Clone Repository') {
            steps {
                checkout scm
            }
        }

        // שלב 2: בניית ה-Docker Image (סעיף 7b בתרגיל)
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${REPO_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        // שלב 3: תיוג ודחיפה ל-ECR (סעיף 7c בתרגיל)
        stage('Push to AWS ECR') {
            steps {
                script {
                    // מתייגים את האימג' המקומי עם הכתובת המלאה של אמזון
                    sh "docker tag ${REPO_NAME}:${IMAGE_TAG} ${ECR_REGISTRY}/${REPO_NAME}:${IMAGE_TAG}"
                    
                    // דוחפים אותו למחסן ה-ECR
                    sh "docker push ${ECR_REGISTRY}/${REPO_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }
}

pipeline {
    agent any

    environment {
        ECR_REGISTRY = "992382545251.dkr.ecr.us-east-1.amazonaws.com"
        REPO_NAME    = "flask-app"
        IMAGE_TAG    = "latest"
        // הזרקה מאובטחת של ה-Token ששלחת לי
        ECR_TOKEN    = "eyJwYXlsb2FkIjoiMEdGaThZbFllVThDTW1SM1JKSXJIb05EZEk3cks1Q1pjTHJpZWIxTWI2SWlGU1VEWUs3U0F0c21wTE9wNkI0bDhhYjRCVWJ4Q0pock1ONlo5QkN2V21nNXJqcVhxdWkzNUgvRWh2WjlxOWxxOWZYOXVCV3BoZythZFRsV29DOW1mVyt1NVF1dHFDUGJhUjJlVFdJa3dPUmV0YVo2N2pmc3pWUG1Za2c5SmFBMG9xMXJSaHJtUHdxdUJGcmNpa2c2VlFwWnhncTZaYmFBdHBXKy83dFd2cmxzZE45U2RVMUkzYU90N3hjUUZhM0p5a2lhTjFVd0xwbkxJcDFCWFA5RWdjMHhkeVN5cnhSNklRaTFTOW9WdzVJOHliMllCZTZiV0JqOU1IT0FHRnd2VVZBb0Jva3RUalFybTFWdkJ1MmRrZFN5dkxpVDNXYzgrdklLdW8wWjJIQllHZXdlRDNTajVWYm9jWHpSUUhjc1ZFelZWMEtlZDBuT3V0NDRIZHM4djlvMmpYSDV0MGRiT1J3RHpWcW51SXhRS2d1dHk4dnNESUFjTldWU2hxS3czcDVBMDZBd01kb1RDNmRMU2o4RVlFUUk3Umt4TGtjQWRLUXVpSlE2M1U2OVMzeTQyTWQ2czNMNUViclhWVDNDV2JKSjBRSEw5R3hqOGg3Q25jcTh6cnVnS1FKMGlYMlZvSmdzc01RcGRxdGVDenIvT2h1RmFHNUROWXFTUEpSeTBUaUp3MFllTVB1QzVnU0xXRFBDdGx5cWdkYzJJaHRXcmtHY1dXeitmUFFCWWlXckIyQTcxRTdWUDJibHB0M1RxWlByaWtnVzkwMnlBSlcxVGVyTzlRdzl2b2dhUnJXVDJyMXVlMjBsc3dSYk5HQmw3UGs3V0FVQkdPTTcyVGlWMnpvcTR5eTR5bHNVWTlQdTRZNmdNeE1MZWhLVnUrZDhtR09FWTBJUVhWWlRwdFB0NmdDOVpFQTVvZDVBRllGNTNPbEI4UVU4a3dZbGRXcDRlbDVHVGt0K0FIYk8rTG9tcW1MZHZuN0dEUXN2TDBRVTFidFVJV1ZnQlhERi85eEZMMjAyaDFvWXpYRnp4NXkvb0FvRk5JM1FVam92MURnbTNwUllkaUFXU0RvSHV5a2V0a2pMYkt0cWtCYkthRXVuSEQrczA5WkdRYnFOaXJESkRGMnZHWW9JN3J0YWk0VzU0L0R2eHZhZFpKejUwVnJjS0hmazJ0S05RU21xckpzY09BVU1lejU5MjV4YUk5eHd0bk5oWHl2akt1M1FQUWtCQm9la21Vb0R6ZG9pbWxhRkZqUnU2Q3ZnSlRSMDZTT2p2OG45UjdpK2p2MFlVc2YzOEtacFFNelFRUzB5UXJ1bGhGV0haYlp6NC9TWXlZMnhycTNHaGRmWHBjdy9FV1o0UUVGK0xHODVlRXdRRWRnTmovcUVFZ2p4NlNEMGxCZHNVWVowSS84ZkZGZW0rRW9PWkJ0NGxUUkVONVhLYUpqMlFMdmpqS21QeWlHMkQ1eEg4SmNMWWNaWCtoU0E1cllsNk1aQUg5eDFBaWdKMW1EMTI2ZXFSYy9RSldPK2tZYTZwV0tINEZ1K2JGS0lEYzNNTTcra3c1SzJxdFcyWnJIOE95b0JKMWlmK3k2VStvOXpNNnA3Ymc9PS"
    }

    stages {
        stage('Clone Repository') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${REPO_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Push to AWS ECR') {
            steps {
                script {
                    // ביצוע לוגין מבוסס משתנה הסביבה המאובטח
                    sh "echo '${ECR_TOKEN}' | docker login --username AWS --password-stdin ${ECR_REGISTRY}"
                    
                    // תיוג ודחיפה של ה-Image ל-ECR
                    sh "docker tag ${REPO_NAME}:${IMAGE_TAG} ${ECR_REGISTRY}/${REPO_NAME}:${IMAGE_TAG}"
                    sh "docker push ${ECR_REGISTRY}/${REPO_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }
}

pipeline {
    agent {
        docker {
            image 'python:3.10'
        }
    }
    
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'pytest test_app.py'
            }
        }
        stage('Deploy') {
            when {
                anyOf {
                    branch 'main'
                    branch pattern: "release/.*", comparator: "REGEXP"
                }
            }
            steps {
                echo "Simulating deploy from branch ${env.BRANCH_NAME}"
            }
        }
    }

    post {
        success {
            script {
                def payload = [
                    content: "✅ Build SUCCESS on `${env.BRANCH_NAME}`\nURL: ${env.BUILD_URL}"
                ]
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson(payload),
                    url: 'hhttps://discord.com/api/webhooks/1372255057206181979/zmm1ZaiJMtwiCF6qOEsvUg5jrR1HhFJioGGmW2KT67xEaW-PxZyavp1Dk75g8CpAwNcS'
                )
            }
        }
        failure {
            script {
                def payload = [
                    content: "❌ Build FAILED on `${env.BRANCH_NAME}`\nURL: ${env.BUILD_URL}"
                ]
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    requestBody: groovy.json.JsonOutput.toJson(payload),
                    url: 'https://discord.com/api/webhooks/1372255057206181979/zmm1ZaiJMtwiCF6qOEsvUg5jrR1HhFJioGGmW2KT67xEaW-PxZyavp1Dk75g8CpAwNcS'
                )
            }
        }
    }
}



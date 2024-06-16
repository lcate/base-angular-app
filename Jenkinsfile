pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'npm install'
                sh 'ng build --configuration production'
                sh 'cd dist && zip -r angular-app.zip . && cd ..'
            }
        }
        stage('Publish to Nexus') {
            steps {
                script {
                    def nexusUrl = "http://nexus:8081/repository/nexus"
                    def artifactPath = "dist/angular-app.zip"
                    def artifactName = "angular-app.zip"
                    def nexusUser = "admin"
                    def nexusPassword = "kate123"

                    withCredentials([usernamePassword(credentialsId: 'nexus-credentials', usernameVariable: 'NEXUS_USERNAME', passwordVariable: 'NEXUS_PASSWORD')]) {
                        sh "curl -u $NEXUS_USERNAME:$NEXUS_PASSWORD --upload-file $artifactPath $nexusUrl/$artifactName"
                    }
                }
            }
        }
    }
}

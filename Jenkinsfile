pipeline {
    agent any

    stages {
        stage('Repo Clone') {
            steps {
                echo "git clone......."
                git url: 'https://github.com/roohmeiy/django-notes-app.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                echo "building......."
                sh "docker build -t notes-app ."
            }
        }

        stage('Push to DockerHub') {
            steps {
                echo "pushing............."
                withCredentials([usernamePassword(credentialsId: 'DockerHub', passwordVariable: 'DockerHubPass', usernameVariable: 'DockerHubUser')]) {
                    sh "docker tag notes-app ${env.DockerHubUser}/notes-app:latest"
                    sh "echo ${env.DockerHubPass} | docker login -u ${env.DockerHubUser} --password-stdin"
                    sh "docker push ${env.DockerHubUser}/notes-app:latest"
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "deploying.............."
                sh "docker-compose down"
                sh "docker-compose up -d"
            }
        }
    }
}

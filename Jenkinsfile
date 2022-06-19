pipeline {
    agent any

    parameters {
        string defaultValue: 'openjdk', description: 'passer l\'image java à utiliser', name: 'image'
        string defaultValue: '11-jdk-slim', description: 'passer le tag de l\'images à utiliser', name: 'tag'
        string defaultValue: 'Allan', description: 'Maintener', name: 'Maitener'
        string defaultValue: 'app.jar', description: 'Nom de l\'application', name: 'appName'
        string defaultValue: 'verkeur08/testapplication', description: 'Nom de l\'image du projet', name: 'newImage'
        string defaultValue: 'latest', description: 'Tag de l\'image du projet', name: 'newTag'
        string defaultValue: 'java', description: 'Type d\'applications ', name: 'typeApp'
    }

    stages {
        
        stage('Récupération du Dockerfiles'){
            steps{
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/asemin08/Dockerfiles.git'
            }
        }
	    stage('Docker pull images') {
            steps {
                sh "docker pull ${params.image}:${params.tag}"
            }
        }
        stage('Docker build Image') {
            steps {
                dir("${params.typeApp}") {
                    sh "docker build --no-cache --build-arg \"image=${params.image}\" --build-arg \"Maitener=${params.Maitener}\" --build-arg \"image=${params.image}\" -t ${params.newImage}:${params.newTag} -f Dockerfile ."
                }
            }
        }
        stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
                 sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                }
            }
        }
        stage('Docker push') {
            steps {
                sh "docker push ${params.newImage}:${params.newTag}"
            }
        }
    }
}

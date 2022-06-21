pipeline {
    agent any

    parameters {
        string defaultValue: 'openjdk', description: 'passer l\'image java à utiliser', name: 'image'
        string defaultValue: '11-jdk-slim', description: 'passer le tag de l\'images à utiliser', name: 'tag'
        string defaultValue: 'Allan', description: 'Maintener', name: 'Maitener'
        string defaultValue: 'verkeur08/testapplication', description: 'Nom de l\'image du projet', name: 'newImage'
        string defaultValue: 'latest', description: 'Tag de l\'image du projet', name: 'newTag'
        string defaultValue: 'java', description: 'Type d\'applications ', name: 'typeApp'
        string defaultValue: 'jar', description: 'Type d\'extension du fichier à cherché ', name: 'extension'
        string defaultValue: '0.0.1-SNAPSHOT', description: 'Version du projet', name: 'version'
        string defaultValue: 'GestionCompteTest', description: 'artifactId du projet', name: 'artifactId'
        string defaultValue: 'eu.esnup', description: 'groupId du projet', name: 'groupId'
        string defaultValue: 'maven-snapshots', description: 'Repository nexus', name: 'repository'
    }

    stages {
	stage('Clean workspace'){
            steps{
            	cleanWs()
            }
        }
        
        stage('Récupération du Dockerfiles'){
            steps{
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/asemin08/Dockerfiles.git'
                dir("${params.typeApp}") {
                    withCredentials([usernameColonPassword(credentialsId: 'nexus-repository', variable: 'nexus')]) {
                        sh "curl -L -u ${nexus} -X GET \"http://146.59.154.110:8081/nexus/service/rest/v1/search/assets/download?sort=version&direction=desc&repository=${params.repository}&maven.groupId=${params.groupId}&maven.artifactId=${params.artifactId}&maven.baseVersion=${params.version}&maven.extension=${params.extension}\" -H \"accept: application/json\" -o app.jar"
                    }
                }
                sh "ls"
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

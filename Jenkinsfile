pipeline {
    agent any

    environment{
        def nodejsTool = tool name: 'Node-20-tool', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
        def dockerTool = tool name: 'docker-latest-tool', type: 'org.jenkinsci.plugins.docker.commons.tools.DockerTool'
        PATH = "${nodejsTool}/bin:${dockerTool}/bin:${env.PATH}"
    }

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Create Production React Build'){
            steps{
                sh 'npm run-script build'
            }
        }

        stage('Build Docker Image') {
            steps {

                withCredentials([usernamePassword(credentialsId: 'personal-docker-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    // Use the USERNAME and PASSWORD environment variables are available within this block  
					sh 'docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}'
                }

                sh '''
                    docker build -t cpierswim/react-jenkins-docker:1 .
                    docker images
                '''

            }
        }

        stage('Push Docker Image') {
            steps {

                withCredentials([usernamePassword(credentialsId: 'personal-docker-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    // Use the USERNAME and PASSWORD environment variables are available within this block  
					sh 'docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}'
                }

                sh 'docker push cpierswim/react-jenkins-docker:1'

            }
        }
    }
}
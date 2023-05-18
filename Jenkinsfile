pipeline {
    agent any

    stages {
        stage('Build') {
            steps {

                script {
                    def nodejsTool = tool name: 'Node-20-tool', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
                    env.PATH = "${nodejsTool}/bin:${env.PATH}"
                }

                sh '''
                    echo "Building the applicaton..."
                    npm install
                    npm run-script build
                '''
            }
        }

        stage('Docker') {
            steps {

                script {
                    def dockerTool = tool name: 'docker-latest-tool', type: 'org.jenkinsci.plugins.docker.commons.tools.DockerTool'
                    env.PATH = "${dockerTool}/bin:${env.PATH}"
                }

                withCredentials([usernamePassword(credentialsId: 'personal-docker-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    // Use the USERNAME and PASSWORD environment variables are available within this block  
					sh 'docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}'
                }

                sh '''
                    echo "Dockerizing the applicaton..."

                    docker build -t cpierswim/react-jenkins-docker:1 .
                    docker images
                    docker push cpierswim/react-jenkins-docker:1
                '''

                // Access the personal-docker-credentials
                // Use them to login to Docker through the login CLI command


            }
        }
    }
}
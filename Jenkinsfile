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
                sh 'echo "Dockerizing the applicaton..."'
            }
        }
    }
}
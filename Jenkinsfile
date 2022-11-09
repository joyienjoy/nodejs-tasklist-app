pipeline {
    agent any
    environment {
        imagename = "joydeep2022/nodejs-tasklist-app"
        registryCredential = 'dockerlogin'
        dockerImage = ''
    }
    stages {
        stage('Git-pull') {
            steps {
                git 'https://github.com/joyienjoy/nodejs-tasklist-app.git'
            }
        }
        stage('Git-build'){
            steps {
                script {
                    dockerImage = docker.build imagename + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Git-push'){
            steps {
                script {
                   docker.withRegistry( '', registryCredential ) {
                   dockerImage.push() }
                }
            }
        }
        stage('Git-deploy'){
            steps {
                script {
                    def remote = [:]
                    remote.name = 'JNode'
                    remote.host = '20.111.50.108'
                    remote.user = 'root'
                    remote.password = '123'
                    remote.allowAnyHosts = true
                    sshCommand remote: remote, command: "docker run -d -p 3000:3000 --name nodeapp $imagename:$BUILD_NUMBER"
                }
            }
        }
    }
}

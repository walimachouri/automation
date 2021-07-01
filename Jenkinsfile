pipeline {
    agent any
    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
        stage('clone project') {
            steps {
                echo 'cloning...'
                git branch: 'main', url: 'https://github.com/walimachouri/automation.git'
            }
        }
        stage('Docker Build and Tag') {
            steps {
                //update repo_id/repo_name (docker-hub)
                sh 'docker build -t samplenginx:latest .' 
                sh 'docker tag samplenginx repo_id/repo_name:latest'
                }
        }
        stage('Push image to docker hub repo') {
            steps {
                withDockerRegistry(credentialsId: 'docker-hub') {
                    sh 'docker push repo_id/repo_name:latest'
                }
                }
            }
        stage('Run Docker container on linux VM') {
            steps {
                // update config name : ssh server creds and repo_id/repo_name (docker hub)
                sshPublisher(publishers: [sshPublisherDesc(configName: 'config-name', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'docker run -it --rm -d -p 80:80  repo_id/repo_name:latest', execTimeout: 0, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
    }
}
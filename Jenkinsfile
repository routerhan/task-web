pipeline {
    agent any
    
    parameters {
        string(name: 'ver', defaultValue: '', description: 'Version')
    }

    stages {
        stage('Build') {
            steps {
                script {
                    sh "echo '<html><body>Version: ${params.ver}</body></html>' > index.html"
                }
            }
        }
        stage('Package') {
            steps {
                script {
                    sh "tar -cvf myapp-${params.ver}.tar index.html"
                }
            }
        }
        stage('Upload to Git') {
            steps {
                script {
                    sh "git add ."
                    sh "git commit -m 'Adding version ${params.ver}.'"
                    sh "git tag -a ${params.ver} -m 'Version ${params.ver}.'"
                    withCredentials([sshUserPrivateKey(credentialsId: 'GITHUB_TASK_SSHKEY', keyFileVariable: 'SSH_KEY_FILE')]) {
                        sh "ssh-agent sh -c 'ssh-add ${SSH_KEY_FILE} && git push git@github.com:routerhan/task-web.git'"
                    }
            }
        }
    }
}

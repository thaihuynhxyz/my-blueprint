pipeline {
    agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: ubuntu
    image: ubuntu:latest
    command:
    - cat
    tty: true
            '''
            defaultContainer 'ubuntu'
        }
    }

    options {
        gitLabConnection('gitlab')
        gitlabBuilds(builds: ['build'])
    }

    stages {
        stage('Say Hello') {
            steps {
                sh 'echo hello world...!'
            }
        }
        stage('for the PR') {
            when {
                branch 'PR-*'
            }
            steps {
                echo 'this on runs for the MRs'
            }
        }
    }

    post {
        success {
            updateGitlabCommitStatus name: 'build', state: 'success'
        }
        failure {
            updateGitlabCommitStatus name: 'build', state: 'failed'
        }
    }
}

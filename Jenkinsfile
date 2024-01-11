pipeline {
    agent {
        docker {
              image '158.160.15.30:8123/jenkinswork/jenkins-agent'
        }
    }

    stages {
        stage('Copy source with configs') {
            steps {
                git(url: 'https://github.com/nickwokeupinseoul/hello1.git', branch: 'patch-1')
            }
        }
        stage('Build war') {
            steps {
                sh 'mvn package'
            }
        }
        stage('make docker image') {
            steps {
                sh 'docker build --tag=webappindocker .'
                sh '''docker tag webappindocker 158.160.15.30:8123/jenkinswork/webappindocker:2-staging && docker push 158.160.15.30:8123/jenkinswork/webappindocker:2-staging'''
            }
        }
        stage('run docker on another serve'){
            steps {
                sh 'ssh-keyscan -H 158.160.7.30 >> ~/.ssh/known_hosts'
                sh '''ssh jenkins@158.160.7.30 << EOF
	                sudo docker pull 158.160.15.30:8123/jenkinswork/webappindocker:2-staging
	                docker run -p 8080:8080 158.160.15.30:8123/jenkinswork/webappindocker:2-staging
                EOF'''
            }  
        }
    }
}

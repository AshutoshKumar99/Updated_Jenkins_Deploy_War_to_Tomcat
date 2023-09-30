pipeline {
    agent {
        docker {
            image 'abhishekf5/maven-abhishek-docker-agent:v1'
            args '--user root -v /var/run/docker.sock:/var/run/docker.sock' // mount Docker socket to access the host's Docker daemon
        }
    }
    stages {
        stage('Git checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/AshutoshKumar99/Updated_Jenkins_Deploy_War_to_Tomcat'
            }
        }

        stage('CodeBuild') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('JUnit Report') {
            steps {
                junit 'target/surefire-reports/*.xml'
            }
        }

        stage('Archive artifact') {
            steps {
                archive "target/**/*.war"
            }
        }

        stage('Deploy WAR to Tomcat') {
            steps {
                sshagent(['new_deploy_user']) {
            sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@3.108.65.55:/opt/apache-tomcat-8.5.93/webapps/'
    }
            }
        }
    }
    
    }

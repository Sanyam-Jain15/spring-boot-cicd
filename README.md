pipeline {
    agent any
    tools {
        maven '3.9.4'
    }
    stages {
        stage('Build Maven') {
            steps {
                // Checkout code from the Git repository
                checkout([$class: 'GitSCM', 
                          branches: [[name: '*/main']], 
                          userRemoteConfigs: [[url: 'https://github.com/Sanyam-Jain15/spring-boot-cicd']]
                         ])
                
                // Determine if the node is Unix or Windows and execute appropriate commands
                script {
                    if (isUnix()) {
                        sh 'mvn clean install'  // For Unix-based agents
                    } else {
                        bat 'mvn clean install' // For Windows agents
                    }
                }
            }
        }
    }
}

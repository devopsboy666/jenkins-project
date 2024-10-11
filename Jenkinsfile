pipeline {
    agent any
    
    tools {nodejs "node_18_19"} 
    
    stages {
        // stage('GIT PULL') {
        //     steps {
        //      git branch: 'main', 
        //         credentialsId: 'jenkins-project', 
        //         url: 'https://github.com/pakawat116688/jenkins-project.git'
        //     }
        // }
        
        stage('SonarQube Analysis') {
            environment { 
                scannerHome = tool 'sonarqube-scanner';
            }
            steps {
                withSonarQubeEnv(credentialsId: 'sonarqube-scan',installationName: 'sonarqube-server' ) {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
        
        stage("Quality gate") {
            steps {
                waitForQualityGate abortPipeline: true
            }
        }
        
        stage('Deploy') {
            steps {
                sh "docker compose up -d web --build"
            }
        }
        
    }
}

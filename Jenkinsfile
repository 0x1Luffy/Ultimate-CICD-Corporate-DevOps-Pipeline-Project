pipeline {
    agent any
    
    tools {
        jdk 'jdk17'
        maven 'maven-3'
    }
    
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git credentialsId: 'GIT_CREDENTIALS_ID', url: 'https://github.com/YOUR_USERNAME/YOUR_REPO.git'
            }
        }
        stage('Compile') {
            steps {
                sh "mvn compile"
            }
        }
        stage('Test') {
            steps {
                sh "mvn test"
            }
        }
        stage('File System Scan') {
            steps {
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SONARQUBE_SERVER') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner \
                            -Dsonar.projectName=BoardGame \
                            -Dsonar.projectKey=BoardGame \
                            -Dsonar.java.binaries=. '''
                }
            }
        }
        stage('Quality Gate') {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'SONARQUBE_CREDENTIALS_ID' 
                }
            }
        }
        stage('Build') {
            steps {
               sh "mvn package"
            }
        }
        
        // Uncomment if you need to publish artifacts to Nexus
        // stage('Publish To Nexus') {
        //     steps {
        //       withMaven(globalMavenSettingsConfig: 'global-settings', jdk: 'jdk17', maven: 'maven-3', traceability: true) {
        //             sh "mvn deploy"
        //         }
        //     }
        // }
        
        stage('Build and Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'DOCKER_CREDENTIALS_ID', toolName: 'docker') {
                        sh "docker build -t YOUR_DOCKER_USERNAME/boardgame:latest ."
                    }
                }
            }
        }
        
        stage('Docker Image Scan') {
            steps {
                sh "trivy image --format table -o trivy-image-report.html YOUR_DOCKER_USERNAME/boardgame:latest"
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'DOCKER_CREDENTIALS_ID', toolName: 'docker') {
                        sh "docker push YOUR_DOCKER_USERNAME/boardgame:latest"
                    }
                }
            }
        }
        
        stage('Deploy to K8s') {
            steps {
                withKubeConfig(credentialsId: 'K8S_CREDENTIALS_ID', namespace: 'webapps', restrictKubeConfigAccess: false) {
                    sh "kubectl apply -f deployment.yml"   
                }
            }
        }
        
        stage('Verify the Deployment') {
            steps {
                withKubeConfig(credentialsId: 'K8S_CREDENTIALS_ID', namespace: 'webapps', restrictKubeConfigAccess: false) {
                    sh "kubectl get pods -n webapps"
                    sh "kubectl get svc -n webapps"
                }
            }
        }
    }
}

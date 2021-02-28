pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    environment{
        GIT_JENK_CRED = credentials('jenkins-git-cred')
    }
    parameters {
        string(name: 'gitURL', defaultValue: 'https://github.com/vishal-choudhari/spring-boot-k8s-azuredevops.git', description: 'Git URL of Source code')
        string(name: 'gitBranch', defaultValue: 'development')
    }
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
        disableConcurrentBuilds()
        timeout(time: 30, unit: 'MINUTES')
    }
    stages {
        stage('Git Checkout') {
            steps{
                sh 'echo "Executing Git Clone'
                git branch: '${gitBranch}', credentialsId: 'jenkins-git-cred', url: '${gitURL}'
            }
        }
        stage('Build binary') {
            steps {
                sh "mvn -B -DskipTests clean package"
            }
        }
        stage('SonarQube analysis') {
            steps{
                withSonarQubeEnv(installationName: 'My SonarQube Server') {
                    sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
                }
            }
        }
    }
}
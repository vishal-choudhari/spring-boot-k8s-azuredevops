pipeline {
    agent any
    
    tools {
        maven 'MAVEN_LOCAL'
        jdk 'JAVA_11'
    }

    parameters {
        string(name: 'git_repo_source', defaultValue: 'https://github.com/vishal-choudhari/spring-boot-k8s-azuredevops.git', description: 'Git repository from where we are going to checkout the code (master branch) and build a docker image. NB! The repository must contain a Dockerfile in the root')
        string(name: 'git_repo_branch', defaultValue: 'main', description: 'The branch to be checked out')
        string(name: 'docker_registry_url', defaultValue: '', description: 'Container Registry URL')
        string(name: 'git_credential_Id', defaultValue: 'github-cred-vishal-choudhari')
        }
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
        disableConcurrentBuilds()
        timeout(time: 10, unit: 'MINUTES')
    }
    stages {
        stage('Checkout git repo') {
            steps {
                    echo "Checking out code"
                    git branch: '${git_repo_branch}',url: '${git_repo_source}', credentialsId: 'github-cred-vishal-choudhari'
            }
        }
        stage('Build binary') {
            steps { 
                   bat 'mvn clean install'
            }
        }
    }
}

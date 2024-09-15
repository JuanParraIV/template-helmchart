pipeline {
    agent any

    stages {
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
            }
        } //Cleanup Workspace
        stage('Checkout from SCM') {
            steps {
                script {
                    git branch: 'main', url: 'https://github.com/JuanParraIV/template-gitops.git', credentialsId: 'github-account'
                    dir('automation') {
                        git branch: 'main', url: 'http://192.168.50.143:3001/jotamario/automationScritps.git', credentialsId: 'gitea-account'
                    }
                }
            }
        } //Checkout from SCM
        stage('Update the Deployment Tags') {
            steps {
                sh './automation/update_helm_values.sh'
            }
        } //Update the Deployment Tags
        stage('Git Operations and Helm Packaging') {
            steps {
                script {
                    // Manejar las operaciones de Git y empaquetado del chart
                    sh 'ls'
                    sh './automation/git_helm_operations.sh'
                }
                withCredentials([gitUsernamePassword(credentialsId: 'github-account', gitToolName: 'Default')]) {
                    sh 'git push https://github.com/JuanParraIV/template-gitops main'
                }
            }
        } //Git Operations and Helm Packaging
    }
}


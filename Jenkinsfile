pipeline {
    agent{
        label "JENKINS-AGENT-1"
    }
    environment {
        APP_NAME = "k8-deployment-test"
        GIT_PAT = 'git-hub'
    }
    
    stages {
        stage('Cleanup Workspace'){
            steps{
                cleanWs()
            }
        }

        stage('SCM Checkout') {
            steps {
                script {
                    git branch: 'main', credentialsId: 'git', url: 'https://github.com/sundayfagbuaro/Q6_deploy.git'
                // cloning repo
                // git credentialsId: 'git', url: 'https://github.com/sundayfagbuaro/Q3_Jenkins_Pipeline.git'
                }
            }
        }

        stage('Update The Deployment Tags'){
            steps{
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }
        }

        stage('Push The Changed Deployment to Git'){
            steps{
                sh """ 
                    git config --global user.name "sundayfagbuaro"
                    git config --global user.email "fabuu4real@gmail.com"
                    git config --global credential "$GIT_PAT"
                    git config --global credential.helper cache
                    git add deployment.yaml
                    git commit -m "updated deployment manifest"
                    git push https://github.com/sundayfagbuaro/Q6_deploy.git main
                """
           //      withCredentials([gitUsernamePassword(credentialsId: 'git', gitToolName: 'Default')]){
           //         sh "git push https://github.com/sundayfagbuaro/Q6_deploy.git main"
           //      }  
           //       withCredentials([(credentialsId: 'git-hub')]){
           //          sh "git push https://${GIT_PAT}@github.com/sundayfagbuaro/Q6_deploy.git main"
                }                   
            }
        

        stage('Deploy to K8s') {
            steps {
                script{
                    kubernetesDeploy (configs: "deployment.yaml", kubeconfigId: 'k8config')
                }
            }
        }
}   } 
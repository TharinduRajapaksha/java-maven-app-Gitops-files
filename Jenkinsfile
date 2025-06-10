pipeline{
    agent{
        label "jenkins-agent"
    }

    environment{
        APP_NAME = "complete-production-e2e-pipeline"
    }

    stages{

        stage("Cleanup Workspace"){
            steps{
                cleanWs() // Clean the workspace before starting the build
            }
        }

        stage("Checkout from SC in GitHub"){
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/TharinduRajapaksha/java-maven-app-Gitops-files' // Checkout the code from the GitHub repository
                checkout scm // Checkout the code from the SCMMm
            }

            
        }

        stage("Update the Deployment Tags"){
            steps{
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}|${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """

            }

            
        }

        stage("Push the Changed Deployment File to GitHub"){
            steps{
                sh """
                    git config --global user.name "TharinduRajapaksha"
                    git config --global user.email "tharindu.tahraka.rajapaksha@gmail.com"
                    git add deployment.yaml
                    git comit -m "Updated deployment file with new image tag"

                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitTeeUsername: 'Default')]) {
                    sh "git push https://github.com/TharinduRajapaksha/java-maven-app-Gitops-files.git HEAD:main"
                
                }
                
            }

            
        }





        
    }
}

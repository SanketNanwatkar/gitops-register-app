pipeline {
    agent { label "Jenkins-Agent" }
    environment {
              APP_NAME = "register-app-pipeline"
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'Github2025', url: 'https://github.com/SanketNanwatkar/gitOps-register-app'
               }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                   cat deployment.yaml
                   sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                   cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                withCredentials([gitUsernamePassword(credentialsId: 'Github2025', gitToolName: 'Default')]) {
                sh '''
                  git config user.name "SanketNanwatkar"
                  git config user.email "sanketnanwatkar@gmail.com"

                  git status
                  git add deployment.yaml
                  git commit -m "Updated Deployment Manifest" || echo "Nothing to commit"

                  git push origin main
                  '''
                }
            
            }
        }
        
    }
}

pipeline {
    
    agent any
    environment {
        REGISTRY_CREDENTIALS = credentials('docker-hub-creds') // Use ID you set for your Docker Hub credentials
        DOCKER_IMAGE = "vikramvimal/cicd-e2e:${BUILD_NUMBER}"
    }
    
    stages {
        
        stage('Checkout'){
           steps {
                sh 'echo passed'
                git credentialsId: 'github',
                url: 'https://github.com/vikramvimal/cicd-end-to-end',
                branch: 'main'
           }
        }
        

        stage('Build and Push Docker Image') {

            steps {
                script{
                    sh 'docker build -t ${DOCKER_IMAGE} .'
                    def dockerImage = docker.image("${DOCKER_IMAGE}")
                    docker.withRegistry('https://index.docker.io/v1/', "docker-hub-creds") {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Checkout K8S manifest SCM'){
            steps {
                git credentialsId: 'github', 
                url: 'https://github.com/vikramvimal/cicd-demo-manifests-repo',
                branch: 'main'
            }
        }

        stage('Update K8S manifest & push to Repo'){
            steps {
                script{
                    withCredentials([usernamePassword(credentialsId: 'github', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        sh '''
                        sed -i "s/v1/${BUILD_NUMBER}/g" deploy.yaml
                        cat deploy.yaml
                        git add deploy.yaml
                        git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
                        git remote -v
                        git push https://github.com/vikramvimal/cicd-demo-manifests-repo.git HEAD:main
                        '''                        
                    }
                }
            }
        }
    }
}

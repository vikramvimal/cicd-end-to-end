pipeline {
    
    agent any
    environment {
        DOCKER_CREDENTIALS = credentials('docker-hub-creds') // Use the ID you set for your Docker Hub credentials
    }
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        
        stage('Checkout'){
           steps {
                git credentialsId: '24f6aa2e-349f-4fa0-ac73-d2c2acf8ab81', 
                url: 'https://github.com/vikramvimal/cicd-end-to-end',
                branch: 'main'
           }
        }
        
        stage('Build and Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS) {
                        // Your Docker build and push commands here
                    }
                }
            }
            
        stage('Build Docker'){
            steps{
                script{
                    sh '''
                    echo 'Buid Docker Image'
                    docker build -t vikramvimal/cicd-e2e:${BUILD_NUMBER} .
                    '''
                }
            }
        }

        stage('Push the artifacts'){
           steps{
                script{
                    sh '''
                    echo 'Push to Repo'
                    docker push vikramvimal/cicd-e2e:${BUILD_NUMBER}
                    '''
                }
            }
        }
        
        // stage('Checkout K8S manifest SCM'){
        //     steps {
        //         git credentialsId: '24f6aa2e-349f-4fa0-ac73-d2c2acf8ab81', 
        //         url: 'https://github.com/vikramvimal/cicd-demo-manifests-repo.git',
        //         branch: 'main'
        //     }
        // }
        
        // stage('Update K8S manifest & push to Repo'){
        //     steps {
        //         script{
        //             withCredentials([usernamePassword(credentialsId: '24f6aa2e-349f-4fa0-ac73-d2c2acf8ab81', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
        //                 sh '''
        //                 cat deploy.yaml
        //                 sed -i '' "s/32/${BUILD_NUMBER}/g" deploy.yaml
        //                 cat deploy.yaml
        //                 git add deploy.yaml
        //                 git commit -m 'Updated the deploy yaml | Jenkins Pipeline'
        //                 git remote -v
        //                 git push https://github.com/vikramvimal/cicd-demo-manifests-repo.git HEAD:main
        //                 '''                        
        //             }
        //         }
        //     }
        // }
    }
}

pipeline{
    agent any
    stages{
        stage("code cloning"){
            steps{
                echo "code cloned from github"
                git branch: 'main', credentialsId: 'gitHub', url: 'https://github.com/mithunku060/devops-django-cicd-pipeline'
            }
        }
        stage("Build"){
            steps{
                echo "Building image from Dockerfile"
                sh "docker build -t my-note-app ."
            }
        }
        stage("docker push"){
            steps{
                echo "Pushing image to dockerhub"
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'docker_pass', usernameVariable: 'docker_user')]){
                    sh "docker tag my-note-app ${env.docker_user}/my-note-app:latest"
                    sh "docker login -u ${env.docker_user} -p ${env.docker_pass}"
                    sh "docker push ${env.docker_user}/my-note-app:latest"
                }
            }
        }
        stage("Deploy to kubernetes"){
            steps{
                        withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'Kubern8', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                        sh "kubectl delete --all pods"
                        sh "kubectl apply -f deployment.yaml"
                        sh "kubectl apply -f service.yaml"
                        }
            }
        }
    }
}

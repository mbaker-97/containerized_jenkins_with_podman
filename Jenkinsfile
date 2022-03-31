pipeline{
    agent any
    stages{
        stage("Git Pull")
        {
            steps{
                git branch: 'main', url: 'https://github.com/mbaker-97/containerized_jenkins_with_podman.git'
                echo "Successful Pull"
            }
        } stage("Podman Build")
        {
            
            steps{
                sh "podman build docker/ -t docker.io/mbaker97/mbaker97:jenkins"
                
            }
        }
        stage("Run Image in Container")
        {
            steps{
                echo "Testing"
                sh "podman run -d --privileged --entrypoint=jenkins --name=jenkins_test docker.io/mbaker97/mbaker97:jenkins --httpPort=79"
            }
        }
        stage("Test Running Container")
        {
            steps{
                echo "Ensure that jenkins process running in container"
                sh "podman exec -it jenkins_test pgrep -af jenkins"
                
                echo "Ensure that container is running"
                sh "podman ps | grep jenkins_test"
                
                echo "Ensure that jenkins is serving over http"
                sh "curl localhost:79 --retry-delay 1 --retry 60 --retry-connrefused"
                
                echo "Ensure that podman is installed"
                sh "podman -v"
            }
        }
        stage("Push to Dockerhub")
        {
            environment{
                DOCKERHUB_CREDS = credentials('dockerhub_creds')
            }
            steps{
                sh 'podman push docker.io/mbaker97/mbaker97:jenkins --creds=$DOCKERHUB_CREDS_USR:$DOCKERHUB_CREDS_PSW'
            }
        }
    }
    post{
        always{
            sh 'podman rm -f jenkins_test'
            deleteDir()
        }
    }
}

pipeline{
    agent any

    stages{
        stage("Test Lint"){
            steps{
                sh "tidy --drop-empty-elements false --drop-empty-paras false -qe app/*.html"
                sh "hadolint Dockerfile"
            }
        }
        stage("Build Docker Image"){
            steps{
                script {
                    capstone_image = docker.build("lbarahona/capstone-udacity")
                }
            }
        }
        stage ('Push Image to Registry') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        capstone_image.push("${env.GIT_COMMIT[0..7]}")
                        capstone_image.push("latest")
                    }
                }
            }
        }
    }
}
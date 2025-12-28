pipeline{
    agent any
    environment{
        docker_credentials = credentials('dockerhub-user-pass')
        image_name = 'sunnah658/myapp'
    }
    stages{
        stage('clone git repo'){
            steps{
                git branch: 'main', url: 'https://github.com/sunnah658/CI_project_1.git'
            }
        }
        stage('build docker image'){
            steps{
                script {
                  sh 'docker build -t $image_name:latest .'  
                }
            }
        }
        stage('push to dockerhub'){
            steps{
                script{
                    sh 'echo $docker_credentials_PSW | docker login -u $docker_credentials_USR --password-stdin'
                    sh 'docker push $image_name:latest'
                }
            }
        }
        stage('deploy'){
            steps{
                script{
                    sh 'docker run -d -p 8081:80 $image_name:latest'
                }
            }
        }
    }
    post{
        success{
            slackSend (channel: 'C0A5QGT2GRK', message: "build successfull: ${JOB_NAME} #${BUILD_NUMBER} ${BUILD_URL}")
        }
         failure{
            slackSend (channel: 'C0A5QGT2GRK', message: "build failed: ${JOB_NAME} #${BUILD_NUMBER} ${BUILD_URL}")
        }
    }
}

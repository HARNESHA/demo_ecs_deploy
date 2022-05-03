pipeline {
    agent any

    environment {
        EC_REGISTRY = "332289956654.dkr.ecr.ap-south-1.amazonaws.com/jay_node_app:latest"
        ECS_CLUSTER = "TEST-ECS-CLUSTER"
    }


    stages {
        stage('info') {
            steps {
                echo "----------------------------------------------------------------------------"
                echo "----------------------retrying workspace information------------------------"
                echo "----------------------------------------------------------------------------"
                sh '''
                    id
                    pwd
                    ls -lah
                '''
            }
        }
        stage('clone') {
            steps {
                echo "----------------------------------------------------------------------------"
                echo "---------------cloing $params.git_repo_http_url from github-----------------"
                echo "----------------------------------------------------------------------------"
                sh script: "rm -rf $params.git_repo_name*"
                sh """
                    pwd
                    ls -alh
                    git clone "$params.git_repo_http_url"
                """
            }
        }
        stage('Build') {
            steps {
                echo "----------------------------------------------------------------------------"
                echo "------building $params.docker_image_name:latest named docker image----------"
                echo "----------------------------------------------------------------------------"
                echo ('Build stages....')
                dir("$params.git_repo_name") {
                    echo 'building docker image ....'
                    sh script: "docker build -t $params.docker_image_name:latest ."
                }
                
            }
        }
        stage('Push') {
            steps {
                echo "----------------------------------------------------------------------------"
                echo "------pushing image $params.docker_image_name:latest to ecr-----------------"
                echo "----------------------------------------------------------------------------"
                sh 'aws ecr get-login-password --region ap-south-1 --profile iam-jay | docker login --username AWS --password-stdin 332289956654.dkr.ecr.ap-south-1.amazonaws.com'
                sh '''
                    docker build -t 332289956654.dkr.ecr.ap-south-1.amazonaws.com/jay_node_app:latest .
                    docker push 332289956654.dkr.ecr.ap-south-1.amazonaws.com/jay_node_app:latest
                   '''
            }
        }
        stage('Deploy') {
            steps {
                 echo ('Deploy stages....')
                sh 'aws ecs update-service --cluster  jay-node-app-cluster --service jay-node-app-service --task-definition jay-node-app-task --force-new-deployment --profile iam-jay'
                echo "----------------------------------------------------------------------------"
                echo "docker container with name $params.container_name deployed sucessfully......"
                echo "----------------------------------------------------------------------------"
                sh script: "docker rmi -f \$(docker images | grep '>' | awk '{print \$3}')"
                echo "old docker image removed sucesssfully."

            }
        }
    }
}
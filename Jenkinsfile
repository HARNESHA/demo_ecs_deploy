pipeline {
    agent any

    environment {
        EC_REGISTRY = "332289956654.dkr.ecr.ap-south-1.amazonaws.com/jay_node_app:latest"
        ECS_CLUSTER = "jay-node-app-cluster"
        ECS_TASK = "jay-node-app-task"
        ECS_SERVICE = "jay-node-app-service"
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
                echo "---------------cloing $params.git_repo_http_url from github-----------------"
                sh script: "rm -rf $params.git_repo_name*"
                sh """
                    pwd
                    ls -alh
                    git clone "$params.git_repo_http_url"
                """
            }
        }
        stage('login') {
            steps {
                echo "---------------------login to ecr-------------------------"
                sh "aws ecr get-login-password --region ap-south-1 --profile iam-jay | docker login --username AWS --password-stdin ${EC_REGISTRY}"
                sh "docker build -t ${EC_REGISTRY} ."
                sh "docker push ${EC_REGISTRY}"   
            }
        }
        stage('build') {
            steps {
                echo "---------------------building docker image------------------------"
                sh "docker build -t ${EC_REGISTRY} ."
            }
        }
        stage('push') {
            steps {
                echo "---------------------pushing image to ecr-------------------------"
                sh "docker push ${EC_REGISTRY}"
            }
        }

        stage('Deploy') {
            steps {
                 echo ('Deploy stages....')
                sh "aws ecs update-service --cluster  ${ECS_CLUSTER} --service ${ECS_SERVICE} --task-definition ${ECS_TASK)} --force-new-deployment --profile iam-jay"
                echo "----------------------------------------------------------------------------"
                echo "docker container with name $params.container_name deployed sucessfully......"
                echo "----------------------------------------------------------------------------"
                sh script: "docker rmi -f \$(docker images | grep '>' | awk '{print \$3}')"
                echo "old docker image removed sucesssfully."

            }
        }
    }
}
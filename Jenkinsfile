pipeline {
    agent any
    stages {
        stage('build-docker-image') {
            steps {
                build_and_push_docker_image()
            }
        }
        stage('deploy-to-dev') {
            steps {
                deploy("dev")
            }
        }
        stage('tests-on-dev') {
            steps {
                run_api_tests("DEV")
            }
        }
        stage('deploy-to-stg') {
            steps {
                deploy("stg")
            }
        }
        stage('tests-on-stg') {
            steps {
                run_api_tests("STG")
            }
        }
        stage('deploy-on-prod') {
            steps {
                deploy("prod")
            }
        }
        stage('tests-on-prod') {
            steps {
                run_api_tests("PRD")
            }
        }
    }
}

def build_and_push_docker_image(){
    echo "Building and pushing the Docker image to Docker Hub"
    sh "docker build --no-cache -t teodorajovcheska7/python-greetings:latest ."

    echo "Push the Docker image to Docker Hub"
    sh "docker push teodorajovcheska7/python-greetings:latest"
}

def deploy(String environment){
    echo "Deploying Python microservice to ${environment} environment"
    sh "docker pull teodorajovcheska7/python-greetings:latest"
    sh "docker-compose stop greetings-app-${environment}"
    sh "docker-compose rm greetings-app-${environment}"
    sh "docker-compose up -d greetings-app-${environment}"
}

def run_api_tests(String environment){
    echo "API tests triggered on ${environment} environment"
    sh "docker pull teodorajovcheska7/api-tests:latest"
    sh "docker run --network=host --rm teodorajovcheska7/api-tests:latest run greetings greetings_${$environment}"
}

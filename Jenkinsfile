pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                buildApp()
            }
        }
        stage('deploy-dev') {
            steps {
                deploy("DEV")
            }
        }
        stage('test-dev') {
            steps {
                test("DEV")
            }
        }
        stage('deploy-stg') {
            steps {
                deploy("STG")
            }
        }
        stage('test-stg') {
            steps {
                test("STG")
            }
        }
        stage('deploy-prd') {
            steps {
                deploy("PRD")
            }
        }
        stage('test-prd') {
            steps {
                test("PRD")
            }
        }
    }
}

def buildApp(){
    echo "Building of node application is starting.."
    sh "docker build -t mtararujs/sample-book-app ."

    echo "Pushing image to docker registry.."
    sh "docker push mtararujs/sample-book-app"
}

def deploy(String environment){
    echo "Deployment of node application on ${environment} environment.."
}

def test(String environment){
    echo "API test executuon against node application on ${environment} environment.."
}
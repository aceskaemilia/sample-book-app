pipeline {
    agent any
    triggers{
        pollSCM('*/1 * * * *')
    }
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
    sh "docker build -t aceskaemilija/sample-book-app ."

    echo "Pushing image to docker registry.."
    sh "docker push aceskaemilija/sample-book-app"
}

def deploy(String environment){
    echo "Deployment of node application on ${environment} environment.."
    sh "docker pull aceskaemilija/sample-book-app"
    // String lowercaseEnv = environment.toLowerCase()
    sh "docker compose stop sample-book-app-${environment.toLowerCase()}"
    sh "docker compose rm sample-book-app-${environment.toLowerCase()}"
    sh "docker compose up -d sample-book-app-${environment.toLowerCase()}"

}

def test(String environment){
    echo "API test executuon against node application on ${environment} environment.."
    sh "docker pull aceskaemilija/api-tests"
    def directory = pwd()
    sh "docker run --rm --network=sample-book-app-network-compose -v '${directory}':/api-tests/mochawesome-report aceskaemilija/api-tests run BOOKS BOOKS_${environment}"
    sh "ls"
    archiveArtifacts allowEmptyArchive: true, artifacts: 'mochawesome.json', followSymlinks: false
}
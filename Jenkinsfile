pipeline {
    environment {
        registry = "vicky228"
        imageName = 'testapp'
        registryCred = 'dockerhub_id'
        gitProject = "https://github.com/viktoriajekor/juice-shop.git"
    }
    agent any
    options {
        timeout(time: 1, unit: 'HOURS')
    }
    stages {
        stage ('preparation') {
            steps {
                deleteDir()
            }
        }
        stage ('get src from git') {
            steps {
                git url: "${gitProject}"
            }
        }
        stage ('run tests') {
            steps {
                echo 'npm run test'
            }
        }
        stage ('build docker') {
            steps {
                script {
                    //pannoi + / + my-juicy-shop = pannoi/my-juicy-shop
                    dockerImage = docker.build("${registry}/${imageName}")
                }
            }
        }
        stage ('docker publish') {
            steps {
                script {
                    docker.withRegistry("${registry}/${imageName}", "${registryCred}") {
                        dockerImage.push()
                    }
                }
            }
        }
        stage ('cleaning') {
            steps {
                sh 'docker rmi -f $(docker images -a -q)'
            }
        }
    }
}

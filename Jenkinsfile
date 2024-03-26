pipeline {
    agent {
        label 'mavenbuilder'
    }
    triggers {
        githubPush()
    }
    environment {
        DOCKER_IMAGE_NAME = 'rgeorgegrid'
        DOCKER_REPO = 'mr'
        DOCKER_REPO_MAIN = 'main'
    }
    stages {
        stage ('Checkstyle') {
            steps {
                script {
                    echo 'RUNNING CHECKSTYLES...'
                    sh 'mvn checkstyle:checkstyle'
                }
            }
        }
        stage ('Test') {
            steps {
                script {
                    echo 'RUNNING TESTS...'
                    sh 'mvn test'
                }
            }
        }
        stage ('Build') {
            steps {
                script {
                    echo 'BUILDING ARTIFACTS...'
                    sh 'mvn clean package'
                }
            }
        }
        stage('Build Docker Image for MR Repository') {
            steps {
                script {
                    def GIT_COMMIT_SHORT = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
                    env.DOCKER_TAG = "${DOCKER_IMAGE_NAME}/${DOCKER_REPO}:${GIT_COMMIT_SHORT}"
                    sh "docker build -t ${DOCKER_TAG} ."
                }
            }
        }        
        stage('Push Docker Image to MR Repository') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_hub_login', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    script {
                        sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                        sh "docker image push ${DOCKER_TAG}"
                    }
                }
            }
        }
        stage('Build Docker Image for Main Branch') {
            when {
                branch 'main'
            }
            steps {
                script {
                    echo 'RUNNING IN MAIN... \n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n'
                    def GIT_COMMIT_SHORT = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
                    env.DOCKER_TAG_MAIN = "${DOCKER_IMAGE_NAME}/${DOCKER_REPO_MAIN}:${GIT_COMMIT_SHORT}"
                    sh "docker build -t ${DOCKER_TAG_MAIN} ."
                }
            }
        }        
        stage('Push Docker Image to Main Repository') {
            when {
                branch 'main'
            }
            steps {
                echo 'RUNNING IN MAIN... \n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n'
                withCredentials([usernamePassword(credentialsId: 'docker_hub_login', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    script {
                        sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                        sh "docker image push ${DOCKER_TAG_MAIN}"
                    }
                }
            }
        }
    }
}

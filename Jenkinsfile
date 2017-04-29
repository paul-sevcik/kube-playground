pipeline {
    agent any
    environment {
        PATH = '/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin'
    }
    stages {
        stage('static code analysis') {
            steps {
                sh '/usr/local/bin/pycodestyle .'
            }
        }
        stage('build docker image') {
            steps {
                sh '''
                    /usr/local/bin/kubectl config use-context minikube
                    eval $(/usr/local/bin/minikube docker-env)
                    if [[ ${JOB_BASE_NAME} == "master" ]]; then
                        version=${BUILD_NUMBER}
                    else
                        version="$(echo ${JOB_BASE_NAME} | sed 's|%2F|-|g')-${BUILD_NUMBER}"
                    fi
                    /usr/local/bin/docker build \
                        -f docker/kube-playground/Dockerfile \
                        -t kube-playground:${version} .
                '''
            }
        }
        stage('deploy docker image') {
            steps {
                sh '''
                    /usr/local/bin/kubectl config use-context minikube
                    eval $(/usr/local/bin/minikube docker-env)
                    if [[ ${JOB_BASE_NAME} == "master" ]]; then
                        version=${BUILD_NUMBER}
                    else
                        version="$(echo ${JOB_BASE_NAME} | sed 's|%2F|-|g')-${BUILD_NUMBER}"
                    fi
                    kubectl set image deployments/kube-playground kube-playground=kube-playground:${version}
                '''
            }
        }
    }
}

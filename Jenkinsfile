pipeline {
    agent any
    stages {
        stage('static code analysis') {
            steps {
                sh 'pycodestyle .'
            }
        }
    }
}

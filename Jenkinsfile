pipeline {
    agent any 
    stages {
        stage('Build') { 
            steps {
                sh 'docker build –tag  test_image  20_04.dockerfile .'
                sh 'docker build –tag  test_image  20_04.dockerfile .'
            }
        }
        stage('Test'){ 
            steps { 
                // 
            }
        }
        stage('Deploy') { 
            steps {
                // 
            }
        }
    }
}

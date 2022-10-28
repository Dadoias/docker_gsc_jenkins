pipeline {
    agent any 
    stages {
        stage('Build') { 
            steps {
                sh 'docker build --tag  test_image-bash  --file 20_04.dockerfile .'
                /*sh 'chmod 777 gsc'
                sh 'chmod 777 gsc.py'
                sh 'chmod 777 config.yaml'*/
                sh 'chmod 777 /home/davide/.jenkins/workspace/gsc_docker_pipeline_example@tmp'
                
                sh './gsc build --insecure-args test_image-bash 20_04.manifest'
                sh 'openssl genrsa -3 -out enclave-key.pem 3072'
            }
        }
        stage('Test'){ 
            steps { 
                sh './gsc sign-image test_image-bash enclave-key.pem' 
            }
        }
        /*stage('Deploy') { 
            steps {
                // 
            }
        }*/
    }
}

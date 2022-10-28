pipeline {
    agent any 
    stages {
        stage('Build') { 
            steps {
                sh 'docker build --tag  test_image-bash  --file 20_04.dockerfile .'
                sh 'chmod -R 755 /home/davide/.jenkins/workspace/gsc_docker_pipeline_example'
                sh './gsc build --insecure-args test_image-bash 20_04.manifest'
                sh 'openssl genrsa -3 -out enclave-key.pem 3072'
            }
        }
        stage('Test'){ 
            steps { 
                sh './gsc sign-image test_image-bash enclave-key.pem' 
            }
        }
        stage('Deploy') { 
            steps {
                /* sh 'docker run --device=/dev/sgx_enclave \
                    -v /var/run/aesmd/aesm.socket:/var/run/aesmd/aesm.socket \
                    <gsc-image_name>-c ls' */

            }
        }
    }
}

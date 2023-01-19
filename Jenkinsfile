properties([parameters([
[$class: 'ChoiceParameter', choiceType: 'PT_SINGLE_SELECT', description: 'Select the docker image to build', filterLength: 1, filterable: false, name: 'Docker_Image', randomName: 'choice-parameter-24910133008274', script: [$class: 'GroovyScript', fallbackScript: [classpath: [], oldScript: '', sandbox: false, script: 'return [\'ERROR\']'], script: [classpath: [], oldScript: '', sandbox: false, script: '''import groovy.json.JsonSlurper
URL docker_image_tags_url = new URL ("https://hub.docker.com/v2/repositories/davideias") 
try {
    def URLConnection http_client = docker_image_tags_url.openConnection() as HttpURLConnection
    http_client.setRequestMethod(\'GET\')
    http_client.connect()
    def dockerhub_response = [:]
    if (http_client.responseCode == 200) {
        dockerhub_response = new JsonSlurper().parseText(http_client.inputStream.getText(\'UTF-8\'))
    } else {
        println("HTTP response error")
        System.exit(0)
    }
    def image_tag_list = []
    dockerhub_response.results.each { tag_metadata -> image_tag_list.add(tag_metadata.name)  }  
    return image_tag_list.sort()
} catch(Throwable t){
    return [t.toString()]
}''']]],

[$class: 'ChoiceParameter', choiceType: 'PT_SINGLE_SELECT', description: 'Build:', filterLength: 1, filterable: false, name: 'Select', randomName: 'choice-parameter-2632125668535', script: [$class: 'GroovyScript', fallbackScript: [classpath: [], oldScript: '', sandbox: false, script: ''], script: [classpath: [], oldScript: '', sandbox: true, script: 'return [\'SGX\', \'no SGX\']']]],

//file(description: 'Select the manifest file to build docker container protected by SGX', name: 'ManifestFile')
])])


    environment {
        SSH='ssh trustup@10.0.0.4'
    }


node {
    stage('Example') {
        def image = "davideias/${Docker_Image}"
        def SSH="ssh trustup@10.0.0.4"
        if ("${Select}" == 'no SGX'){

            //sh "az login --identity -u 4bcb1222-9b64-4d45-b6f9-9819b741233a"
            //sh "az acr login -n trustupcontainerregistry "
            sh "docker pull ${image}"
            sh "docker run ${image}"

        }else{
            def fileBase64 = input message: 'Inserire ManifestFile per la configurazione del container', parameters: [base64File('file')]
            withEnv(["fileBase64=$fileBase64"]) {
                sh 'base64 -d > ManifestFile.manifest'
                // powershell '[IO.File]::WriteAllBytes("myFile.txt", [Convert]::FromBase64String($env:fileBase64))'
            }

            //sh "$SSH az login --identity -u 1319d00b-1a4d-42a2-9f9b-b9ddd2ad9244"
            //sh "$SSH az acr login -n trustupcontainerregistry "

            // sh “$SSH chmod -R 755 /home/davide/.jenkins/workspace/Active_choice_folder”
            sh "$SSH docker pull ${image}"   
            sh "$SSH ./gsc build --insecure-args ${image} ManifestFile.manifest"
            sh "$SSH openssl genrsa -3 -out key.pem 3072"
            sh "$SSH ./gsc sign-image ${image}-unsigned key.pem"
           // sh "./gsc sign-image gsc-${image}-unsigned ${key}"
            sh "$SSH docker run --device=/dev/sgx_enclave \
                    -v /var/run/aesmd/aesm.socket:/var/run/aesmd/aesm.socket \
                    gsc-${Docker_Image} -c ls"

        }
    }
}

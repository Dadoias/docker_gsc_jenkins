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


node {
    stage('Example') {
        if ("${Select}" == 'no SGX'){
            sh "docker run davideias/${Docker_Image}"
        }else{
            def myFileParam = input message: 'Please provide a file', parameters: [base64File('file')]
            
            /*withFileParameter('fileBase64') {
                sh 'cat $fileBase64'
            }*/
            /*withEnv(["fileBase64=$fileBase64"]) {
                //sh "cat $fileBase64 | base64 -d"
                sh 'echo $fileBase64 | base64 -d > myFile.txt'
                // powershell '[IO.File]::WriteAllBytes("myFile.txt", [Convert]::FromBase64String($env:fileBase64))'
            }*/
            wrap([$delegate: parameters.myFileParam]) {
                sh 'cat myFileParam'
            }

            
            //echo "Hello ${firstName}"
            /*sh "./gsc build --insecure-args ${Docker_Image} ${manifest}"
            sh "./gsc sign-image gsc-${Docker_Image}-unsigned ${key}"
            sh "docker run --device=/dev/sgx_enclave \
                    -v /var/run/aesmd/aesm.socket:/var/run/aesmd/aesm.socket \
                    gsc-${Docker_Image} -c ls"*/
        }
    }
}

/*def fileBase64 = input message: 'Please provide a file', parameters: [base64File('file')]
node {
    withEnv(["fileBase64=$fileBase64"]) {
        sh 'echo $fileBase64 | base64 -d > myFile.txt'
        // powershell '[IO.File]::WriteAllBytes("myFile.txt", [Convert]::FromBase64String($env:fileBase64))'
    }
    // do something with the file stored in ./myFile.txt
}*/



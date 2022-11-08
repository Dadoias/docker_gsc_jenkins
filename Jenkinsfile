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

file(description: 'Select the manifest file to build docker container protected by SGX', name: 'ManifestFile')
])])


pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
                echo "${Docker_Image}"
                //echo "params.Docker_Image"
            }
        }
    }
}




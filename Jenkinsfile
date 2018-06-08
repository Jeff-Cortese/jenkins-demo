#!groovy

node('crowsnest') {
    checkout scm

    try {
        def mainBuild = { ->
            def nodeImage = docker.image('node:8.11.2')

            stage('build') {
                nodeImage.inside { ->
                    sh "npm run build"
                }
            }
        }

        mainBuild()
    } catch(ex) {
        println ex
        throw ex;
    }
}
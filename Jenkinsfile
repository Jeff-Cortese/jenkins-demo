#!groovy

node('crowsnest') {
    checkout scm

    try {
        def mainBuild = { ->
            def nodeImage = docker.image('node:8.11.2')

            nodeImage.inside { ->
                stage('build') {
                    sh "npm install"
                    sh "npm run build"
                }

                stage('Test') { ->
                    withEnv(["CI=true"]) { ->
                        sh "CI=true && npm test"
                    }
                }
            }

        }

        mainBuild()
    } catch(ex) {
        println ex
        throw ex;
    }
}